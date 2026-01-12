```yaml
number: 16509
title: "[red-knot] Several failing tests for generics"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/generic-tests
created_at: 2025-03-04T20:31:36Z
updated_at: 2025-03-06T07:55:31Z
url: https://github.com/astral-sh/ruff/pull/16509
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Several failing tests for generics

---

_@dcreager_

To kick off the work of supporting generics, this adds many new (currently failing) tests, showing the behavior we plan to support.

This is still missing a lot!  Not included:

- typevar tuples
- param specs
- variance
- `Self`

But it's a good start!  We can add more failing tests for those once we tackle these.

---

_Label `red-knot` added by @dcreager on 2025-03-04 20:31_

---

_Review requested from @carljm by @dcreager on 2025-03-04 20:31_

---

_Review requested from @MichaReiser by @dcreager on 2025-03-04 20:31_

---

_Review requested from @AlexWaygood by @dcreager on 2025-03-04 20:31_

---

_Review requested from @sharkdp by @dcreager on 2025-03-04 20:31_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:89 on 2025-03-04 20:54_

I did not understand this part. Did you intend to write `c.x`?

---

_@sharkdp reviewed on 2025-03-04 20:54_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:12 on 2025-03-04 20:56_

What is the principle by which this is `int` and not `Literal[1]`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:22 on 2025-03-04 20:56_

Similarly, what makes this not `Literal[True]`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:27 on 2025-03-04 20:56_

Or `Literal["string"]`?

---

_@sharkdp reviewed on 2025-03-04 20:58_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:116 on 2025-03-04 20:58_

Maybe use something other than `float` here, due to the special case for `float` and `complex`? I don't think anything is wrong with the assertion — we would probably show `E[float]` — but it could be confusing because specifying `E[float]` in a type expression would be equivalent to `E[float | int]`.

---

_@dcreager reviewed on 2025-03-04 20:58_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:89 on 2025-03-04 20:58_

I sure did!  Fixed

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:108 on 2025-03-04 21:01_

We should probably implement checking of returned types against the return type annotation soon :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:116 on 2025-03-04 21:03_

Agreed -- though it may also be that `float` was chosen intentionally as a builtin type without literals, to dodge the question of whether `E(1)` is `E[int]` or `E[Literal[1]]`...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/scoping.md`:63 on 2025-03-04 21:06_

`reveal_type` on these two return types, showing that there's no cross-contamination (e.g. we don't infer `int | str` or something)

---

_@sharkdp reviewed on 2025-03-04 21:18_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:158 on 2025-03-04 21:18_

Ha! I recently tried if CRTP was possible in Python. :smile:

I don't think it would work like this, as the base of `Sub` is eagerly evaluated as a non-type expression. So even `from __future__ import annotations` wouldn't help the fact that `Sub` is not yet defined when you try to specify it as it's own base.

I'm not sure if you need CRTP in Python, given that it has `Self`?
```py
from typing import Self

class Base:
    def f(self) -> Self: ...

class Sub(Base): ...

reveal_type(Sub().f())  # revealed: Sub
```

---

_@sharkdp reviewed on 2025-03-04 21:22_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:1 on 2025-03-04 21:22_

I'll probably not be able to finish my review of this PR today, but I did create a `pep695_type_aliases.md` test suite a while back, so it probably makes sense to check if there is any overlap (unless you already did).

Also, you might want to add
````markdown
```toml
[environment]
python-version = "3.12"
```
````
here, or @ntBre will soon make your tests fail :smile: 

---

_@sharkdp reviewed on 2025-03-04 21:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:47 on 2025-03-04 21:37_

By "the return type", do you mean "types of `return` expressions" or similar?

I think "must be an instance of the actual typevar" is too narrow? Shouldn't it be something like "if a function is annotated as returning a typevar `T`, types of `return` expressions must be assignable to `T`, and not …"?

---

_@sharkdp reviewed on 2025-03-04 21:41_

Haven't finished the review, but this looks like a great start — thank you.

The old `mdtest/generics.md` test is something that I had to update quite a few times when making seemingly unrelated changes, so I'm kind of glad that it's gone. I hope that the "TODO" assertions in the tests here won't introduce similar problems. I guess we'll find out, but it looks like you already made the assertions rather non-specific (e.g. without precise error messages), which is good.

---

_@ntBre reviewed on 2025-03-04 21:51_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:1 on 2025-03-04 21:51_

Don't worry too much! I backed out the red-knot integration for now. I probably won't get back to #16379 this week even. If this merges first, I'm happy to clean up the test failures.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:158 on 2025-03-04 21:58_

`Self` may work for some CRTP use cases, but this recursive pattern is used in Python, and we need to support it. (Most notably, `str` inherits `Sequence[str]` in typeshed.) That's why we had a test for this in the old `generics.md`, which I assume this test was inspired by / replaces.

The two ways to make this code work are to a) put it in a stub file (where all annotations, and base class expressions, are lazy), or b) stringify the inner `Sub` (e.g. `class Sub(Base["Sub"]): ...`), which type checkers do understand (although it does lead to some extra indirection for anyone trying to introspect the type arguments at runtime).

I think we should expand this test to show that it works in a stub, it works with the stringified reference, and it does not work (emits an undefined-name diagnostic) if used as shown.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/scoping.md`:66 on 2025-03-04 21:58_

"Class methods" is within confusion range of "Classmethods", which are a different thing :)
```suggestion
## Methods can mention class typevars
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/scoping.md`:88 on 2025-03-04 21:59_

```suggestion
## Methods can mention other typevars
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/scoping.md`:128 on 2025-03-04 22:00_

```suggestion
This is true with the legacy syntax:
```

---

_@carljm approved on 2025-03-04 22:02_

Looks great!!

---

_@carljm reviewed on 2025-03-04 22:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:47 on 2025-03-04 22:08_

I think this whole sentence is an attempt to clarify what "assignable to T" means. That is, if `T` has an upper bound of `int`, that does not make `int` assignable to `T`.

---

_Review request for @MichaReiser removed by @MichaReiser on 2025-03-05 07:40_

---

_@sharkdp reviewed on 2025-03-05 08:09_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:1 on 2025-03-05 08:09_

I understand that these test suites are not meant to be exhaustive yet, but it might make sense to add a few tests that may inform the design of the constraint solver? (similar to what you did with the *Inferring “deep” generic parameter types* test)

For example, a case which can not be solved by simply solving constraints eagerly for the first parameter, then for the second, …

For example:

```py
def f[T](x: T, y: T) -> T:
    return x

reveal_type(f("a", "b"))  # str
reveal_type(f("a", 1))  # str | int
```

Or this

```py
def f[T](x: T | int, y: T) -> T:
    return y

reveal_type(f(1, "a"))  # str
reveal_type(f("a", "a"))  # str
reveal_type(f(1, 1))  # int
reveal_type(f("a", 1))  # str | int
```

or this

```py
def f[T, S](x: T | S, y: tuple[T, S]) -> tuple[T, S]:
    return y

reveal_type(f("a", ("a", 1)))  # tuple[str, int]
reveal_type(f(1, ("a", 1)))  # tuple[str, int]
```
(interesting: Pyright infers `tuple[str | int, int]` in the very last row here, which is also correct, but seems to imply some asymmetry in constraint solving?)

And maybe also add some generic function definitions that are not allowed:

```py
def absurd[T]() -> T: ...
```

Another thing that seems worth including is a test that makes sure that we can solve nested calls of generic functions (and don't conflate two occurrences of `T` in different functions), e.g.:

```py
def f[T](x: T) -> tuple[T, int]:
    return (x, 1)

def g[T](x: T) -> T | None:
    return x

reveal_type(f(g("a")))  # tuple[str | None, int]
reveal_type(g(f("a")))  # tuple[str, int] | None
```

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:12 on 2025-03-05 18:38_

In [the RFC](https://www.notion.so/RFC-generic-functions-and-classes-13b48797e1ca803aad1ccc79268088c3?d=13c48797e1ca80d59f37001c67ea5bcf&pvs=4#13c48797e1ca80108f94d9626bb13986) it seemed like we hadn't landed on which one we would want it to be, but that mypy and pyright both infer this as `int`.  I put `int` not as a strong opinion about which result we want, but rather because I needed to put something and consistency with mypy/pyright seemed like a good mild tiebreaker.  I can instead put both in the TODO along with a comment that we haven't actually decided yet.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:22 on 2025-03-05 18:39_

Ditto above, updated the TODO to list both possibilities

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:27 on 2025-03-05 18:39_

Ditto above, updated the TODO to list both possibilities

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:116 on 2025-03-05 18:42_

Is it fair to say that even if we infer `E[Literal[1]]` up above in the generic function example, for classes it's more likely that we'd want to infer the weaker `E[int]`?  (For now I've listed both possibilities like above)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/scoping.md`:63 on 2025-03-05 18:43_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:158 on 2025-03-05 18:43_

> The two ways to make this code work are to a) put it in a stub file

That would explain why it was in a stub file in the RFC :joy: 

> I think we should expand this test to show that it works in a stub, it works with the stringified reference, and it does not work (emits an undefined-name diagnostic) if used as shown.

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:47 on 2025-03-05 18:58_

> By "the return type", do you mean "types of `return` expressions" or similar?

It should have been "the returned type".

I copied this example from the RFC. I think what it's trying to check also applies to the parameters, which might be clearer with some `reveal_type`s in the body.  i.e. something like

```py
reveal_type(x)  # revealed: T & int
```

instead of 

```py
reveal_type(x)  # revealed: int
```

I took a stab at rewording this, lmkwyt

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:1 on 2025-03-05 19:33_

> but it might make sense to add a few tests that may inform the design of the constraint solver?

These are great examples, thank you!  Added them all



---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:1 on 2025-03-05 19:34_

> And maybe also add some generic function definitions that are not allowed:
> 
> ```python
> def absurd[T]() -> T: ...
> ```

pyright doesn't consider this a separate class of error, it reports "typevar only appears once, just use `object`", just like for

```py
def f[T](x: T) -> None: ...
```

What about something like

```py
def f[T]() -> list[T]:
    return []
```

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:1 on 2025-03-05 19:37_

> Another thing that seems worth including is a test that makes sure that we can solve nested calls of generic functions (and don't conflate two occurrences of `T` in different functions), e.g.:

Done

---

_@dcreager reviewed on 2025-03-05 19:39_

---

_@carljm reviewed on 2025-03-05 20:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:12 on 2025-03-05 20:17_

Shoot, I was hoping you'd have a carefully reasoned strong opinion here 😆 Mentioning both options in the TODO seems good for now.

---

_@carljm approved on 2025-03-05 20:32_

LGTM!

---

_Merged by @dcreager on 2025-03-05 22:21_

---

_Closed by @dcreager on 2025-03-05 22:21_

---

_Branch deleted on 2025-03-05 22:21_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:132 on 2025-03-06 07:51_

I'm going to have to change this test in my attribute-access PR, because `x: T` is only a declaration, which means that we can access `x` on instances of `Base[T]`, but not on class objects.

I'll probably change it to something like
```suggestion
class Base[T]:
    x: T | None = None
```

---

_@sharkdp reviewed on 2025-03-06 07:51_

---

_@sharkdp reviewed on 2025-03-06 07:55_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:47 on 2025-03-06 07:55_

> I took a stab at rewording this, lmkwyt

I think it's clear now, thank you.

---
