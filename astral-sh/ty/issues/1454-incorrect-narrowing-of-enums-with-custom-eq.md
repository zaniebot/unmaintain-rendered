---
number: 1454
title: "Incorrect narrowing of enums with custom `__eq__` methods in `match` statements"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - narrowing
  - runtime semantics
  - enums
assignees: []
created_at: 2025-10-29T19:32:49Z
updated_at: 2026-01-01T13:43:39Z
url: https://github.com/astral-sh/ty/issues/1454
synced_at: 2026-01-10T01:51:14Z
---

# Incorrect narrowing of enums with custom `__eq__` methods in `match` statements

---

_Issue opened by @AlexWaygood on 2025-10-29 19:32_

### Summary

Originally reported in https://discuss.python.org/t/amend-pep-586-to-make-enum-values-subtypes-of-literal/59456/9.

At runtime, `case` statements that use [literal patterns](https://peps.python.org/pep-0634/#literal-patterns) or [value patterns](https://peps.python.org/pep-0634/#value-patterns) dispatch based on equality, and `Color.GREEN == "g"` in the following example (since `Color` has `str` in its MRO as well as `Enum`, and inherits its `__eq__` method from `str`). We don't recognise this correctly in ty currently:

```py
from enum import StrEnum
from typing import Literal, assert_never, reveal_type

class Color(StrEnum):
    RED = "r"
    GREEN = "g"
    BLUE = "b"

def test_literal_as_enum(x: Literal["g"]) -> None:
    match x:
        case Color.RED:
            assert_never(x)
        case Color.GREEN:
            reveal_type(x)  # this branch is taken at runtime
        case Color.BLUE:
            assert_never(x)
        case _:
            assert_never(x)  # false-positive error here

def test_enum_as_literal(y: Literal[Color.BLUE]) -> None:
    match y:
        case "r":
            assert_never(y)
        case "g":
            assert_never(y)
        case "b":
            reveal_type(y)  # this branch is taken at runtime
        case _:
            assert_never(y)  # false-positive error here
```

https://play.ty.dev/e106f66f-339a-4d1f-a6e4-95f8a4f3bed9

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-10-29 19:32_

---

_Label `narrowing` added by @AlexWaygood on 2025-10-29 19:32_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-10-29 19:32_

---

_Label `enums` added by @AlexWaygood on 2025-10-29 19:32_

---

_Comment by @AlexWaygood on 2025-10-29 19:39_

Similarly, we also incorrectly narrow the type for other `Enum` classes that override `__eq__`. Here we say that the type of `e` must be `Literal[Color.RED]` in the `case Color.RED` branch. But at runtime, that branch is also taken if you pass in `Color.GREEN`, so this inference is incorrectly precise. It's unsafe to do any narrowing at all if the enum class has an arbitrary user-defined `__eq__` method:

```py
from enum import StrEnum, Enum
from typing import Literal, assert_never, reveal_type

class Color(Enum):
    RED = "r"
    GREEN = "g"

    def __eq__(self, other) -> Literal[True]:
        return True

def enum_with_overridden_eq(e: Color):
    match e:
        case Color.RED:
            print("it's red")
            reveal_type(e)
        case Color.GREEN:
            reveal_type(e)
        case _:
            reveal_type(e)

enum_with_overridden_eq(Color.GREEN)
```

https://play.ty.dev/711252af-30bc-4c5b-9a61-44965e2d06da

---

_Renamed from "Incorrect narrowing of `StrEnum`s in `match` statements" to "Incorrect narrowing of enums with custom `__eq__` methods in `match` statements" by @AlexWaygood on 2025-10-29 19:40_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-11-24 22:07_

---

_Comment by @thepetertessier on 2025-12-31 16:57_

Maybe this belongs in its own issue, but I noticed a similar problem when narrowing plain strings. As a simple example (https://play.ty.dev/ff93caef-1263-4286-9600-ba58f95bafc1):

```python
from typing import Literal


def convert_to_bool(bool_str: Literal["True", "False"]):
    return bool_str == "True"

def main(string: str):
    match string:
        case "True" | "False":
            return convert_to_bool(string)  # incorrectly throws invalid-argument-type: Expected `Literal["True", "False"]`, found `str`
```

---

_Comment by @AlexWaygood on 2026-01-01 13:43_

@thepetertessier that's unrelated to this issue -- but it's covered by https://github.com/astral-sh/ty/issues/1566

---
