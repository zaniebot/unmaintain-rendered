```yaml
number: 17059
title: "[red-knot] Narrowing on `in tuple[...]` and `in str`"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: narrowing-in-tuples
created_at: 2025-03-29T23:54:17Z
updated_at: 2025-04-01T23:50:20Z
url: https://github.com/astral-sh/ruff/pull/17059
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Narrowing on `in tuple[...]` and `in str`

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Part of #13694

Seems there a bit more to cover regarding `in` and other types, but i can cover them in different PRs

## Test Plan
Add `in.md` file in narrowing conditionals folder


---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-29 23:54_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-29 23:54_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-29 23:54_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-29 23:54_

---

_Comment by @github-actions[bot] on 2025-03-29 23:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Converted to draft by @MatthewMckee4 on 2025-03-30 00:13_

---

_Renamed from "[red-knot] Narrowing on `in tuple[...]`" to "[red-knot] Narrowing on `in tuple[...]` and `in str`" by @MatthewMckee4 on 2025-03-30 00:14_

---

_Comment by @MatthewMckee4 on 2025-03-30 00:33_

Okay, i realise this is not right
```python
from typing import Literal

def _(x: Literal["a", "b", "c", "d"]):
    if x in "abc":
        reveal_type(x)  # revealed: Literal["a", "b", "c"]
    else:
        reveal_type(x)  # revealed: Literal["d"]
```

The true type is Literal["a", "b", "c", "ab", "ac", "bc", "abc"]. So i should just infer as str right?
        
Edit: In fact im wrong in this instance, but if the argument x was str then i think this is the case?

---

_Marked ready for review by @MatthewMckee4 on 2025-03-30 00:50_

---

_Comment by @MatthewMckee4 on 2025-03-30 02:33_

Im a bit unsure about if we should support other types in str. For example we have these right now:
```py
def _(x: Literal["a"]) -> None:
    if x in "abc":
        reveal_type(x) # revealed: Literal["a"]
```
```py
def _(x: Literal["a", "b"]) -> None:
    if x in "abc":
        reveal_type(x) # revealed: Literal["a", "b"]
```
```py
def _(x: str) -> None:
    if x in "abc":
        reveal_type(x) # revealed: str
```
but we also have:
```py
def _(x: object) -> None:
    if x in "abc":
        reveal_type(x) # revealed: Literal["a", "b", "c"]
```
*we also get [unsupported-operator] error here
but i feel like this should be str.

My thinking for all StringLiteral and Union containing only StringLiteral, we can narrow the type, but otherwise we can only infer as str? How does this sounds. 

---

_Label `red-knot` added by @AlexWaygood on 2025-03-30 05:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/in.md`:12 on 2025-03-30 18:14_

This narrowing in the `if` branch here is not sound. `x` could be a subclass of `int` with arbitrary `__eq__` behavior that makes the `in` test return `True`:

```py
>>> class MyInt(int):
...     def __eq__(self, other):
...             return True
...
>>> MyInt(7) in (1, 2)
True
```

Basically we cannot do positive narrowing on `in` tests or equality checks unless the subject type is a literal type or union of literal types, because of the possibility of a subclass with custom `__eq__` behavior.

The exclusions in the negative branch here are valid (whatever `x` is, we know it can't be any of `Literal[1, 2, 3]`, because those would definitely return `True` on the `in` test). Not sure how much practical value this negative narrowing would provide, or whether it's worth the added complexity or the more-complex types.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/in.md`:25 on 2025-03-30 18:17_

This is also not sound, for effectively the same reason as above: `in` tests use equality, which can be customized via `__eq__` to do anything at all, so the fact that `x` compares equal to either an instance of `A` or an instance of `B` tells us nothing about the type of `x`.

In this case the negative narrowing is even less sound; it's perfectly normal (the default behavior, even) for two different instances of the same type to not compare equal to each other. So `x` not being present in `(A(), B())` definitely does not tell us that `x` cannot be of type `A` or `B`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/in.md`:61 on 2025-03-30 18:19_

Similarly to the first case above, because `str` can be subclassed and a subclass can implement arbitrary `__eq__`, we cannot do this narrowing in the positive branch. The negative narrowing here is valid (whatever `x` is, we know it can't be `Literal["a"]` etc) but may not be worth it.

---

_@carljm requested changes on 2025-03-30 18:52_

Haven't reviewed the code yet, just the tests, but there are some things we'll have to be a bit more conservative about here.

---

_@MatthewMckee4 reviewed on 2025-03-30 19:19_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/in.md`:25 on 2025-03-30 19:19_

Okay yeah I see why this is bad, I'll remove this test for now then

---

_Comment by @MatthewMckee4 on 2025-03-30 19:20_

Okay thank you, I'll look over this again

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-03-31 12:18_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:304 on 2025-03-31 18:42_

`UnionType::from_elements` is a less verbose way to do this

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:307 on 2025-03-31 18:44_

This case is identical to the above case, we can just `||` the conditions instead of duplicating the arms. Or even wrap the conditions up in a single method, which I think we could reasonably call `Type::is_closed`. (That is, the type is a singleton or a union of singletons, or equivalent to a union of singletons, which would be true of e.g. `bool` or an enum type also.) I think the implementation of this method could also use `Type::is_singleton` rather than `Type::is_literal`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:295 on 2025-03-31 18:46_

The only use of this argument is to negate the type in each arm, can't we just do that once at the call-site and eliminate this argument?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:337 on 2025-03-31 18:46_

This is a confusing and inefficient way to do a no-op, instead let's have this method return `Option<Type<'db>>` and just return `None` here; if the caller gets back `None` it just doesn't insert a constraint.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:327 on 2025-03-31 18:48_

These two cases can also be collapsed into one, and the guard test can just use the same `Type::is_closed` method discussed above.

---

_@carljm reviewed on 2025-03-31 18:48_

The behavior here looks good! I think we can simplify the implementation and make it a bit more general.

---

_Comment by @MatthewMckee4 on 2025-03-31 20:08_

Thank you 

---

_@MatthewMckee4 reviewed on 2025-03-31 20:11_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/narrow.rs`:295 on 2025-03-31 20:11_

We don't always negate though, only for certain arms

---

_@MatthewMckee4 reviewed on 2025-03-31 20:13_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/narrow.rs`:307 on 2025-03-31 20:13_

Okay thank you, I'm still not aware of all of the methods in the 'Type' enum so this is very useful thank you 

---

_@carljm reviewed on 2025-03-31 20:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:295 on 2025-03-31 20:30_

If we eliminate the "do nothing" arm by just returning `None` instead, as I suggested below, then I think all the remaining arms do negate?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:307 on 2025-03-31 20:31_

I think I mis-spoke here, what we'd want is `is_single_valued()` not `is_singleton()`

---

_@carljm reviewed on 2025-03-31 20:31_

---

_@MatthewMckee4 reviewed on 2025-03-31 21:15_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/narrow.rs`:327 on 2025-03-31 21:15_

dont we just want to check this if the first type is a literal string or union of literal strings, because then we can narrow it down, like:

```py
from typing import Literal

def _(x: Literal["a", "b", "c", "d"]):
    if x in "abc":
        reveal_type(x)  # revealed: Literal["a", "b", "c"]
    else:
        reveal_type(x)  # revealed: Literal["d"]
```

and we can only do this if its `str in str` i think.

---

_@carljm reviewed on 2025-03-31 21:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:327 on 2025-03-31 21:23_

No it's correct to do it more broadly, consider a case like this:

```
def _(x: Literal[1, 2, "a", "b", "c", "d"]):
    if x in "abc":
        reveal_type(x)  # revealed: Literal["a", "b", "c"]
```

The type of `x` here is not solely string literals, but we still can and should narrow here. Even if `x` were purely int literals, we'd want to reveal `Never` there. (We should probably add a couple additional tests with mixed types and totally-non-string types like this, checking for containment in a string literal.)

---

_@MatthewMckee4 reviewed on 2025-03-31 21:30_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/narrow.rs`:327 on 2025-03-31 21:30_

Sorry i want to fully understand this, but mypy and pyright both say revealed type is `Literal[1, 2, 'a', 'b', 'c', 'd']`, and also both error with "Operator "in" not supported for types "Literal[1, 2, 'a', 'b', 'c', 'd']" and "Literal['abc']"".

Also the program fails when we do _(1).

Would you be able to explain more why we should narrow the type when there would be a runtime error.

Okay maybe if we have an example like this, would this be correct, since if x was 1 or 2, the program would fail?
```python
def _(x: Literal[1, 2, "a", "b", "c"]):
    if x in "abc":
        reveal_type(x)  # revealed: Literal["a", "b", "c"]
    reveal_type(x)  # revealed: Never
```

Maybe im completely wrong here so any explanation is very helpful, thanks

---

_@MatthewMckee4 reviewed on 2025-03-31 21:53_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/narrow.rs`:327 on 2025-03-31 21:53_

Sorry, i can understand you, i think i was overthinking things, ill change the code

---

_@MatthewMckee4 reviewed on 2025-03-31 21:59_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/narrow.rs`:327 on 2025-03-31 21:59_

Do you agree with this test:
```py
from typing import Literal

def _(x: Literal[1, 2, "a", "b", "c", "d"]):
    # error: [unsupported-operator] "Operator `in` is not supported for types `int` and `str`, in comparing `Literal[1, 2, "a", "b", "c", "d"]` with `Literal["abc"]`"
    if x in "abc":
        reveal_type(x)  # revealed: Literal["a", "b", "c"]
    else:
        reveal_type(x)  # revealed: Literal["d"]
```

---

_@carljm reviewed on 2025-03-31 23:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:327 on 2025-03-31 23:03_

Ugh, sorry, I didn't think carefully enough. I do agree with that test, but I also think it's not important that we narrow in an error comparison case, and we shouldn't go out of our way to do it if it doesn't fall out naturally.

---

_@carljm reviewed on 2025-03-31 23:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:327 on 2025-03-31 23:26_

I pushed some changes to more efficiently clarify the implementation I was looking for. It doesn't quite match your test in that we don't eliminate the unsupported-operation values from the union in the else clause, but I think this isn't important enough to add extra complexity for.

I also decided that `Type::is_closed` didn't add clarity with only one call site.

---

_@MatthewMckee4 reviewed on 2025-03-31 23:28_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/narrow.rs`:327 on 2025-03-31 23:28_

Thank you, looks good

---

_@carljm approved on 2025-03-31 23:29_

---

_Merged by @carljm on 2025-03-31 23:38_

---

_Closed by @carljm on 2025-03-31 23:38_

---

_Branch deleted on 2025-04-01 23:50_

---
