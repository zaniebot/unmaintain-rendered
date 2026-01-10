```yaml
number: 99
title: Missed exhaustiveness checking for class unions
type: issue
state: closed
author: charliermarsh
labels:
  - narrowing
assignees: []
created_at: 2025-05-03T16:33:58Z
updated_at: 2025-12-01T11:37:31Z
url: https://github.com/astral-sh/ty/issues/99
synced_at: 2026-01-10T01:58:59Z
```

# Missed exhaustiveness checking for class unions

---

_Issue opened by @charliermarsh on 2025-05-03 16:33_

`print(obj_type)` here is marked as "used when possibly not defined".

```python
class Foo:
    pass

class Bar:
    pass

class Baz:
    pass

FooBarBaz = Foo | Bar | Baz


def func(obj: FooBarBaz):
    match obj:
        case Foo():
            obj_type = "Foo"
        case Bar():
            obj_type = "Bar"
        case Baz():
            obj_type = "Baz"
    
    print(obj_type)
```

See: https://types.ruff.rs/74979412-252c-4f0a-a271-1c4fb1e6bc82

---

_Comment by @AlexWaygood on 2025-05-03 18:01_

There are two missing features here. The first is that we don't understand the legacy type alias at all (we have a pretty good understanding of PEP-695 type aliases, but we currently only understand very simple legacy type aliases such as `X = int`).

But even if you rewrite it as `type FooBarBaz = Foo | Bar | Baz`, we still don't understand the `match` statement as being exhaustive. And it seems like the problem persists if you use `if/elif` as well: https://types.ruff.rs/7d05802c-0dcf-4e5f-8c81-c4f778d8d1c1

---

_Comment by @carljm on 2025-05-03 19:16_

In both the if/elif and match cases, we do all the narrowing correctly and we understand that if you add a `case _: ...` or an `else:`, the type of `obj` within it is `Never`. But we don't yet make use of that understanding to prune the possible control flow paths.

---

_Label `narrowing` added by @carljm on 2025-05-08 04:44_

---

_Comment by @jaseemabid on 2025-05-22 11:48_

I'm seeing the same errors for Enums as well, probably the same underlying issue. 

https://play.ty.dev/8afe9230-2803-4392-bf6d-0f23462867ad

```python
from enum import Enum

class Pet(Enum):
    cat = "cat"
    dog = "dog"


def speak(p: Pet) -> str:
    match p:
        case Pet.cat:
            return "meow"
        case Pet.dog:
           return "woof"
 #     case _:
 #         return "This shouldn't be needed"

```

☝ has one false positive error

```text
Function can implicitly return `None`, which is not assignable to return type `str` (invalid-return-type) [Ln 8, Col 22]
```

---

_Comment by @AlexWaygood on 2025-05-22 15:02_

@jaseemabid we don't really support enums at all yet unfortunately, I'm afraid -- you'd need both this feature and https://github.com/astral-sh/ty/issues/183 to get rid of the diagnostic there

---

_Comment by @kohlivarun5 on 2025-06-04 03:01_

Just to record interest, I'm seeing the same issue as @jaseemabid for enums 

---

_Comment by @sharkdp on 2025-06-17 09:28_

Supporting exhaustiveness checks in `match` statements would probably involve adding a new `NotNever(<expr>)` reachability constraint type, very similar to the one that is used to resolve #180. We could then report a `NotNever(match_subject)` constraint in the (implicit or explicit) catch-all case of the match statement.

---

_Comment by @carljm on 2025-06-17 15:26_

@sharkdp Same approach should work for if/elif/else chains, I think?

---

_Comment by @sharkdp on 2025-06-17 15:44_

> Same approach should work for if/elif/else chains, I think?

My thought was that this is harder because we need to extract the narrowing subject from the test expressions, and it might not be safe to do so in all cases? In the example below, it seems reasonable to detect that `x` is the subject:
```py
def _(x: Literal[0, 1]):
    if x == 0:
        pass
    elif x == 1:
        pass
    else:
        reveal_type(x)  # Never
```

But what if we have something like this (stupid example)? Do we extract all identifiers that have appeared in `if` or `elif` conditions prior to the `else` case, and put a `NotNever` constraint on all of them (and *AND* them together)?
```py
def _(x: Literal[0, 1]):
    ZERO = 0
    ONE = 1

    if x == ZERO:
        pass
    elif x == ONE:
        pass
    else:
        # reachable if `x` is not Never AND `ZERO` is not Never AND `ONE` is not Never.
        reveal_type(x)  # Never
```

---

_Comment by @carljm on 2025-06-17 15:54_

Makes sense.

Is there an alternative approach here where we instead check whether the last branch (whether an `elif` or a `case`) is statically known to be taken? Isn't this guaranteed to be the case for any exhaustive `match` or `if/elif` chain? The obvious case is that the final clause is a `case _:` or an `else:`, which trivially is known to be taken. But if we don't have one of those, then if the checks are exhaustive we should be able to see that the final `elif` test is statically-known-true, or that the final `case` is statically known to match.

(I'm not sure that this is simpler than your idea for `match`, but I think it is for `if/elif`?)

---

_Comment by @sharkdp on 2025-06-17 17:58_

> Is there an alternative approach here where we instead check whether the last branch (whether an `elif` or a `case`) is statically known to be taken? Isn't this guaranteed to be the case for any exhaustive `match` or `if/elif` chain?

Yes, you're right. I originally came here because I thought that we should already support this. But then I somehow confused myself, probably because I tested with `match` instead of `if`. In fact, we do already support "exhaustiveness checks" in easy cases:

```py
def _(x: Literal[0, 1]):
    if x == 0:
        pass
    elif x == 1:
        pass
    else:
        assert_never(x)  # this passes, `x` is narrowed to `Never`

        # we also detect this branch as unreachable, because `x == 1` is
        # inferred as `Literal[True]` in the `elif` case with `x` narrowed to
        # `Literal[0, 1] & ~Literal[0] = Literal[1]`

```

Something like the following does not work, but we could probably make it work by inferring literal Boolean types for `isinstance(…)`:

```py
class A: ...
class B: ...

def _(x: A | B):
    if isinstance(x, A):
        pass
    elif isinstance(x, B):
        pass
    else:
        assert_never(x)  # this passes, `x` is narrowed to `Never`
        # but this branch is not detected as unreachable, because
        # `isinstance(x: A & ~B, B)` is not inferred as `Literal[True]`.
```

For some reason, the following also doesn't work, so I guess it might just be an incorrect modelling of reachability constraints in `match` statements?

```py
def matching(x: Literal[0, 1]):
    match x:
        case 0:
            pass
        case 1:
            pass
        case _:
            assert_never(x)  # this passes, `x` is narrowed to `Never`

            # not detected as unreachable
```

> But if we don't have one of those, then if the checks are exhaustive we should be able to see that the final `elif` test is statically-known-true, or that the final `case` is statically known to match.

👍 

In summary: Something like this `NotNever(<expr>)` constraint is not needed, but we might have to improve type *inference* for some expressions that we can already handle in narrowing (`isinstance`, `issubclass`, type guards(?), …). And it looks like we need to look at the reachability constraint modelling in `match` statements again.

---

_Comment by @sharkdp on 2025-06-18 07:41_

I think the problem with `match` statements is the following:
```py
def matching(x: Literal[0, 1]):
    match x:
        case 0:
            pass
        case 1:
            pass
        case _:
            # reachable?
```

When we evaluate the `case 1` pattern, we compare the type of the subject expression (the `x` in `match x`) with the type of the value pattern (`1`). But since we only store a reference to the `x` expression at the top of the `match` statement, narrowing constraints that come from previous patterns are not taken into account when inferring types for `x`. This means that we still see `Literal[0, 1]` as the type of the subject expression when evaluating `case 1`, and so we infer an ambiguous truthiness.

Instead, we should re-evaluate the subject expression *under the current narrowing constraints*. One way to do this might be to inject/record the subject expression repeatedly for each `case`. This way, we would get new "uses" of `x` with the right narrowing constraints. There might be better ways to solve this, though.

---

_Comment by @carljm on 2025-07-19 00:22_

> Instead, we should re-evaluate the subject expression _under the current narrowing constraints_.

It seem like we shouldn't need to re-evaluate the subject expression anew each time -- the type of the subject expression itself never changes (it is only evaluated once). What changes is our understanding of the constraints on its possible type, given that we've reached match case N.

And I also think that re-evaluating the subject expression under the current narrowing constraints might fail to give us exhaustiveness detection when the subject is a more complex expression (not a simple Place). Because in that case there are no applicable narrowing constraints (narrowing applies only to Places).

It seems like we might need more of a new system here? Where we track the accumulated set of constraints on the type of the subject at each match arm, and the reachability constraints that we add to the control flow paths include those constraints, as well as the original subject expression (which we would still evaluate only once, to its original type).

---

_Comment by @erictraut on 2025-07-19 01:43_

Yes, you'll likely need to create a new system here. That's what I needed to do in pyright. So did the Adrian Freund, the contributor who implemented this support for mypy. Interestingly, Adrian did this as his bachelors thesis for his CS degree at KIT. If you can read German or don't mind using a translation program, his [thesis](https://archive.org/details/typinferenz-und-vollstandigkeitsuberprufung-fur-structural-pattern-matching-in-python) is an interesting read.

---

_Comment by @sharkdp on 2025-07-23 17:16_

> It seems like we might need more of a new system here? Where we track the accumulated set of constraints on the type of the subject at each match arm, and the reachability constraints that we add to the control flow paths include those constraints, as well as the original subject expression (which we would still evaluate only once, to its original type).

Thank you @carljm. This paragraph helped me to find a solution.

---

_Closed by @sharkdp on 2025-07-23 20:45_

---

_Comment by @sasanjac on 2025-12-01 11:23_

This issue still persists when matching against classes that use generics: 

```python
class Ok[T]:
    def __init__(self, value: T) -> None:
        self._value = value


class Err[E]:
    def __init__(self, value: E) -> None:
        self._value = value


def get_data() -> Ok[str] | Err[str]:
    return Ok("example")


def func() -> str:
    match get_data():
        case Ok():
            return "Foo"
        case Err():
            return "Bar"
```

> Function can implicitly return `None`, which is not assignable to return type `str`


---

_Comment by @sharkdp on 2025-12-01 11:37_

@sasanjac Thank you. I opened #1702 to discuss this.

---
