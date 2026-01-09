---
number: 14012
title: "[red-knot] No diagnostic emitted for iterating over objects which might or might not be iterable"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
created_at: 2024-10-31T11:23:47Z
updated_at: 2025-02-20T12:50:09Z
url: https://github.com/astral-sh/ruff/issues/14012
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] No diagnostic emitted for iterating over objects which might or might not be iterable

---

_Issue opened by @AlexWaygood on 2024-10-31 11:23_

As spotted by @MichaReiser in https://github.com/astral-sh/ruff/pull/13992#pullrequestreview-2404711897, red-knot does not currently emit diagnostics if you attempt to iterate over an object that might be iterable, but also might not. For example, we should emit diagnostics on all of the following snippets, but currently do not:

### Union where one element has no `__iter__` method:

<details>

```py
class TestIter:
    def __next__(self) -> int:
        return 42

class Test:
    def __iter__(self) -> TestIter:
        return TestIter()

def coinflip() -> bool:
    return True

for x in Test() if coinflip() else 42:  # should trigger a diagnostic, we don't currently
    pass
```

</details>

### Union type as _iterable_ where one union element has invalid `__iter__` method

<details>

```py
class TestIter:
    def __next__(self) -> int:
        return 42

class Test:
    def __iter__(self) -> TestIter:
        return TestIter()

class Test2:
    def __iter__(self) -> int:
        return 42

def coinflip() -> bool:
    return True

for x in Test() if coinflip() else Test2():  # should trigger a diagnostic, we don't currently
    pass
```

</details>

### Union type as _iterator_ where one union element has no `__next__` method

<details>

```py
class TestIter:
    def __next__(self) -> int:
        return 42

class Test:
    def __iter__(self) -> TestIter | int:
        return TestIter()

for x in Test():  # should trigger a diagnostic, we don't currently
    pass
```

</details>

These are surprisingly hard to fix with our current architecture! I spent several hours yesterday attempting to fix them, and my conclusion is that while it is _possible_ to fix them with our current design, it's messy and complicated enough that we should pursue some refactors first.

One of the issues is the design of our `CallOutcome` enum, which is returned from `Type::call`. `Type::iterate()` calls `CallOutcome::return_ty` to determine what the iterable's `__iter__` and the iterator's `__next__` methods return. `CallOutcome::return_ty` is supposed to help abstract over the fact that the type you're trying to "call" might not be callable (and this includes `Type::Unbound`): it returns `None` if this is the case, or `Some(ty)` if the type you're trying to call is callable. If the type you're trying to call is a union between callable and not-callable types, however, `Some(ty)` is returned; there's no signal to the caller of `CallOutcome::return_ty` that the call could fail if the runtime type of the variable is a member of one of the union elements that was not callable.

https://github.com/astral-sh/ruff/blob/76e4277696e8b34b771d554f8f0d07054d413289/crates/red_knot_python_semantic/src/types.rs#L1424-L1450

(This is precisely the situation we need to detect in the above snippets. If `__iter__` might be callable, but also might not, then the variable might be iterable, but also might not, which means that we need to emit a diagnostic saying that the code trying to iterate over the maybe-iterable variable is potentially unsafe.)

It seemed like calling `CallOutcome::return_ty` is not the solution to this problem, so I explored other solutions that instead directly pattern matched on the possible variants of `CallOutcome`. This is possible, but messy and hard. For a start, `CallOutcome` has a dedicated variant just for `reveal_type`, which means that if you're matching over the possible variants of `CallOutcome`, you are forced to explicitly consider whether a type should be revealed by red-knot in cases like these:

```py
from typing_extensions import reveal_type

class Foo:
    __iter__ = reveal_type

for var in Foo(): ...  # should this... cause us to reveal a type?!
                       # (`__iter__` is "called" on the iterable as part of the `for` loop!)

class Bar:
    __next__ = reveal_type

class Baz:
    def __iter__() -> Bar:
        return Bar()

for var2 in Baz(): ...  # should this... cause us to reveal a type?!
                        # (`__next__` is "called" on the iterator as part of the `for` loop!)
```

I suppose from a purist perspective, this _is_ something we should explicitly consider, but it doesn't... feel like a useful use of my time (and adds extra complexity) to explicitly consider it!

Another issue with attempting to explicitly match over the possible variants of a `CallOutcome` instance is that `CallOutcome::Union` is a recursive variant, meaning you have to write a recursive function just to match over it -- e.g. here's a function I wrote for a partial solution that fixes the first two false-negatives above but not the third one:

<details>
<summary>Annoyingly complicated recursive function matching on `CallOutcome` variants</summary>

```rs
fn call_dunder_next_on_iterator<'db>(
    db: &'db dyn Db,
    dunder_iter_outcome: &CallOutcome<'db>,
    original_iterable: Type<'db>,
) -> IterationOutcome<'db> {
    match dunder_iter_outcome {
        CallOutcome::Callable {
            return_ty: iterator_ty,
        } => {
            let dunder_next_method = iterator_ty.to_meta_type(db).member(db, "__next__");

            dunder_next_method
                .call(db, &[*iterator_ty])
                .return_ty(db)
                .map(|element_ty| {
                    IterationOutcome::Single(IterationOutcomeInner::Iterable { element_ty })
                })
                .unwrap_or(IterationOutcome::Single(
                    IterationOutcomeInner::NotIterable {
                        not_iterable_ty: original_iterable,
                    },
                ))
        }
        CallOutcome::NotCallable { not_callable_ty: _ } => {
            IterationOutcome::Single(IterationOutcomeInner::NotIterable {
                not_iterable_ty: original_iterable,
            })
        }
        CallOutcome::RevealType { .. } => panic!("Why??"),
        CallOutcome::Union {
            called_ty,
            outcomes,
        } => {
            let mut iteration_outcomes = vec![];
            for outcome in outcomes {
                match call_dunder_next_on_iterator(db, outcome, *called_ty) {
                    IterationOutcome::Single(outcome) => iteration_outcomes.push(outcome),
                    IterationOutcome::Union {
                        iterable_ty: _,
                        outcomes,
                    } => iteration_outcomes.extend(outcomes),
                }
            }
            IterationOutcome::Union {
                iterable_ty: original_iterable,
                outcomes: iteration_outcomes.into_boxed_slice(),
            }
        }
    }
}
```

</details>

I think there are ways to avoid making `CallOutcome::Union` non-recursive, and that we should pursue them.

All told, I think we should not attempt to fix these bugs for now until we've improved our code structure. A lot of this will look pretty different after @sharkdp's work to remove `Type::Unbound` in https://github.com/astral-sh/ruff/pull/13980, so I don't plan on looking into this anymore until that's landed.

---

_Label `bug` added by @AlexWaygood on 2024-10-31 11:23_

---

_Label `red-knot` added by @AlexWaygood on 2024-10-31 11:23_

---

_Referenced in [astral-sh/ruff#13992](../../astral-sh/ruff/pulls/13992.md) on 2024-10-31 11:24_

---

_Referenced in [astral-sh/ruff#14016](../../astral-sh/ruff/pulls/14016.md) on 2024-10-31 12:45_

---

_Comment by @AlexWaygood on 2024-10-31 12:49_

https://github.com/astral-sh/ruff/pull/14016 adds some failing tests with TODO comments to illustrate our current limitations.

---

_Comment by @AlexWaygood on 2024-10-31 12:52_

See also #13996, for some other issues with `CallOutcome`

---

_Referenced in [astral-sh/ruff#13980](../../astral-sh/ruff/pulls/13980.md) on 2024-10-31 15:23_

---

_Comment by @carljm on 2024-10-31 17:32_

I think an alternative approach in cases like this, where the correct handling of a call to a union is complex, is to avoid calling union types altogether, and instead map over the union at a higher level. So wrap up the logic involving calling a dunder in a method, and if we see a union, call that method for every element in the union and fold the results back into a union.

---

_Referenced in [astral-sh/ruff#16161](../../astral-sh/ruff/pulls/16161.md) on 2025-02-14 15:13_

---

_Comment by @AlexWaygood on 2025-02-20 12:50_

We now emit diagnostics in all the appropriate places here. There are still some issues, but they deserve their own new issue; this is now out of date. Closing as completed 🥳

---

_Closed by @AlexWaygood on 2025-02-20 12:50_

---
