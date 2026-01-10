---
number: 1060
title: "Rules for not exhaustive `match` case statements"
type: issue
state: open
author: crispyricepc
labels:
  - lint
assignees: []
created_at: 2025-08-20T12:56:22Z
updated_at: 2026-01-09T02:27:10Z
url: https://github.com/astral-sh/ty/issues/1060
synced_at: 2026-01-10T01:48:23Z
---

# Rules for not exhaustive `match` case statements

---

_Issue opened by @crispyricepc on 2025-08-20 12:56_

### Summary

If a `match` statement is used with an enum type or some other type with a finite number of possible values, Ruff (or Ty, not sure which works better here) should warn about unused cases.

## Invalid code

```py
from enum import Enum

enum Animal(Enum):
    CAT = 0
    DOG = 1

animal = get_animal()

match animal: # warning should appear here
    case CAT:
        print("is a cat")
```

## Valid code

```py
from enum import Enum

enum Animal(Enum):
    CAT = 0
    DOG = 1

animal = get_animal()

match animal:
    case CAT:
        print("is a cat")
    case _:
        pass
```

This is inspired by a similar rule in pyright: `reportMatchNotExhaustive`

---

_Comment by @MichaReiser on 2025-08-20 13:10_

This makes sense, but is something that is difficult for Ruff because it will require multifile analysis in most instances (and a good understanding of what values `animal` can expand to). 

That's why I'll move this issue to ty. It's a sub issue of https://github.com/astral-sh/ty/issues/887

---

_Label `lint` added by @MichaReiser on 2025-08-20 13:10_

---

_Comment by @sharkdp on 2025-08-20 13:16_

It potentially makes sense to add this as a separate opt-in rule, but in general, there is nothing wrong with a non-exhaustive match statement in Python.

If you want a fine-granular opt-in to exhaustiveness checking, you can already do this today with ty, by using [`assert_never`](https://typing.python.org/en/latest/guides/unreachable.html#assert-never-and-exhaustiveness-checking):

https://play.ty.dev/8644b344-867a-487c-ba13-ca595f310549
```py
from enum import Enum
from typing import assert_never

class Animal(Enum):
    CAT = 0
    # uncomment the following line, to see the error appear:
    # DOG = 1

def _(animal: Animal):
    match animal:
        case Animal.CAT:
            print("is a cat")
        case _:
            assert_never(animal)
```

In many cases, the `assert_never` branch is not even needed for exhaustiveness checking. For example, if you had a function like the following, [you would already get an error](https://play.ty.dev/7909996d-bbff-47f1-a271-62737256bf6b) when a new enum variant is added, because we would then infer that the function can implicitly return `None`:
```py
def get_animal_name(animal: Animal) -> str:
    match animal:
        case Animal.CAT:
            return "cat"
```

---

_Comment by @AlexWaygood on 2025-08-20 13:20_

Mypy implements this as [an opt-in error code](https://mypy.readthedocs.io/en/stable/error_code_list2.html#check-that-match-statements-match-exhaustively-exhaustive-match). I believe pyright's version (`reportMatchNotExhaustive`) is opt-out. I agree with @sharkdp that to me it seems too opinionated for it to be opt-out: it's more of a lint rule informed by type information than a typing error _per se_. Failing to do an exhaustive match will often lead to typing errors, but we'll already catch those and tell you about them, as @sharkdp says.

---

_Comment by @LoicRiegel on 2025-09-09 06:54_

Hi, I ran into some issues as well.

Look at the following code. I would expect ty to understand that the pattern matching covers all cases, but.

- ``test_method``: ty does not understand the type of self (linked to #159).
- ``test_method_2``: ty correctly understand that everything is okay. Interestingly, mypy does not.
- ``test_method_3``: ty throws an error (maybe also linked to #159 ?). This time, mypy correctly throws no error.

```py
from __future__ import annotations

from enum import Enum
from typing import Self, reveal_type


class TestEnum(Enum):
    LOW = 1
    MEDIUM = 2
    HIGH = 3

    def test_method(self) -> str:
        # reveal_type(self)  # ty: Unknown. Mypy: TestEnum
        match self:
            case TestEnum.LOW:
                return "A"
            case TestEnum.MEDIUM:
                return "B"
            case TestEnum.HIGH:
                return "C"

    def test_method_2(self: TestEnum) -> str:
        # reveal_type(self)  # ty: TestEnum. Mypy: TestEnum
        match self:
            case TestEnum.LOW:
                return "A"
            case TestEnum.MEDIUM:
                return "B"
            case TestEnum.HIGH:
                return "C"

    def test_method_3(self: Self) -> str:
        # reveal_type(self)  # ty: Self@test_method_3. Mypy: Self`0
        match self:
            case TestEnum.LOW:
                return "A"
            case TestEnum.MEDIUM:
                return "B"
            case TestEnum.HIGH:
                return "C"
```

```
> uvx ty check tmp/test_enum.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                                  error[invalid-return-type]: Function can implicitly return `None`, which is not assignable to return type `str`
  --> tmp\test_enum.py:12:30
   |
10 |     HIGH = 3
11 |
12 |     def test_method(self) -> str:
   |                              ^^^
13 |         # reveal_type(self)  # ty: Unknown. Mypy: TestEnum
14 |         match self:
   |
info: rule `invalid-return-type` is enabled by default

error[invalid-return-type]: Function can implicitly return `None`, which is not assignable to return type `str`
  --> tmp\test_enum.py:32:38
   |
30 |                 return "C"
31 |
32 |     def test_method_3(self: Self) -> str:
   |                                      ^^^
33 |         # reveal_type(self)  # ty: Self@test_method_3. Mypy: Self`0
34 |         match self:
   |
info: rule `invalid-return-type` is enabled by default

Found 2 diagnostics
```
```
t> uvx mypy tmp\test_enum.py
tmp\test_enum.py:32: error: Missing return statement  [return]
Found 1 error in 1 file (checked 1 source file)
```


---

_Comment by @sharkdp on 2025-09-09 07:02_

@LoicRiegel Thank you. None of this is really related to `match` statements though. `test_method` is #159. ~~And `test_method3` is #1124 (will post a comment there that the issue is more general than described in the ticket).~~

---

_Comment by @LoicRiegel on 2025-09-09 07:05_

@sharkdp I still wanted to mention those issues here, in case other people find themselves in the same problem as me. This might save them some investigation time

---

_Comment by @sharkdp on 2025-09-09 07:09_

Yeah, no worries. Actually, maybe `test_method3` is more interesting than I thought?
```py
class TestEnum(Enum):
    LOW = 1
    MEDIUM = 2
    HIGH = 3

    def test_method_3(self: Self) -> str:
        reveal_type(self)  # ty: Self@test_method_3. Mypy: Self`0

        # […]
```

Usually, `self: Self` (or an implicit `self`) means that `self` could be of type `TestEnum` or any subtype of it. But enum classes are implicitly final, so does it make sense to infer `self: TestEnum` here? (and similarly, infer `self: F` for all final classes `F`?)

Edit: mypy seems to do that.

---

_Comment by @AlexWaygood on 2025-09-09 07:15_

> Yeah, no worries. Actually, maybe `test_method3` is more interesting than I thought?
> 
> class TestEnum(Enum):
>     LOW = 1
>     MEDIUM = 2
>     HIGH = 3
> 
>     def test_method_3(self: Self) -> str:
>         reveal_type(self)  # ty: Self@test_method_3. Mypy: Self`0
> 
>         # […]
> Usually, `self: Self` (or an implicit `self`) means that `self` could be of type `TestEnum` or any subtype of it. But enum classes are implicitly final, so does it make sense to infer `self: TestEnum` here? (and similarly, infer `self: F` for all final classes `F`?)

Yes, I think it makes sense to eagerly solve the Self TypeVar if its upper bound is a final class!

---
