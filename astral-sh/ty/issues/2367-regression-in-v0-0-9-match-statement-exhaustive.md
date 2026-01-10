---
number: 2367
title: "Regression in v0.0.9: match statement exhaustive check of enum values"
type: issue
state: closed
author: scosman
labels: []
assignees: []
created_at: 2026-01-06T16:12:11Z
updated_at: 2026-01-06T21:10:26Z
url: https://github.com/astral-sh/ty/issues/2367
synced_at: 2026-01-10T01:51:14Z
---

# Regression in v0.0.9: match statement exhaustive check of enum values

---

_Issue opened by @scosman on 2026-01-06 16:12_

### Summary

The latest release has regressed exhaustive enum checking. In V0.0.8 it would know the `case _:` statement was unreachable, and not complain. However, in v0.0.9 it complains, even though it's unreachable.

I use this pattern to ensure all enum cases are covered. Complaining if it's reachable is the goal: type checker forces me to handle new enum values in the match statement.


## Minimal Repo

```python
from enum import Enum
from typing import NoReturn

from pydantic import BaseModel, Field


class TestEnum(str, Enum):
    A = ("a",)
    B = ("b",)
    C = ("c",)


class TestClass(BaseModel):
    def __init__(self, value: TestEnum):
        self._value = value

    value: TestEnum = Field(
        description="A field.",
    )

# Note: also reproduces with assert_never. I use this function so the type checker complains unless all enum values are handled.
def raise_exhaustive_enum_error(value: NoReturn) -> NoReturn:
    raise ValueError(f"Unhandled enum value: {value}")


def isA(item: TestClass | None) -> bool:
    if item is None:
        return False
    val = item.value
    match item._value:
        case TestEnum.A:
            return True
        case TestEnum.B:
            return False
        case TestEnum.C:
            return False
        case _:
            raise_exhaustive_enum_error(val)
            # Also reproduces with assert_never

```


## V0.0.9 Behaviour

Complains about the typing, even though it's unreachable (exhaustive enum check above).

```
39 |             raise_exhaustive_enum_error(val)
   |                                         ^^^ Expected `Never`, found `TestEnum | Unknown`
```

or using assert_never

```
40 |             assert_never(val)
   |             ^^^^^^^^^^^^^---^
   |                          |
   |                          Inferred type of argument is `TestEnum`
```

## V0.0.8 Behaviour

`All checks passed!`

### Version

0.0.9

---

_Comment by @AlexWaygood on 2026-01-06 21:10_

`git bisect` points to 

```
9333f1543378b4ebb1b8098bd16eec0de2032247 is the first bad commit
commit 9333f1543378b4ebb1b8098bd16eec0de2032247
Author: Charlie Marsh <charlie.r.marsh@gmail.com>
Date:   Mon Dec 29 22:19:28 2025 -0500

    [ty] Fix match exhaustiveness for enum | None unions (#22290)
    
    ## Summary
    
    If we match on an `TestEnum | None`, then when adding a case like
    `~Literal[TestEnum.FOO]` (i.e., after `if value == TestEnum.FOO:
    return`), we'd distribute `Literal[TestEnum.BAR]` on the entire builder,
    creating `None & Literal[TestEnum.BAR]` which simplified to `Never`.
    Instead, we should only expand to the remaining members for pieces of
    the intersection that contain the enum.
    
    Now, `(TestEnum | None) & ~Literal[TestEnum.FOO] &
    ~Literal[TestEnum.BAR]` correctly simplifies to `None` instead of
    `Never`.
    
    Closes https://github.com/astral-sh/ty/issues/2260.

 .../resources/mdtest/conditional/match.md          | 49 +++++++++++++++++
 crates/ty_python_semantic/src/types/builder.rs     | 63 ++++++++++++++++------
 2 files changed, 96 insertions(+), 16 deletions(-)
```

...and, now that I look at your code more closely, I think that this is a desirable change. You haven't annotated the `_value` attribute on your `TestClass` class, so the inferred public type of this attribute is not `TestEnum`, it is `TestEnum | Unknown`. The error on your code goes away if you change your `TestClass` to either this:

```py
class TestClass(BaseModel):
    def __init__(self, value: TestEnum):
        self._value: TestEnum = value

    value: TestEnum = Field(
        description="A field.",
    )
```

or this:

```py
class TestClass(BaseModel):
    _value: TestEnum

    def __init__(self, value: TestEnum):
        self._value: TestEnum = value

    value: TestEnum = Field(
        description="A field.",
    )
```

You can see my explanation in https://github.com/astral-sh/ty/issues/2370 for why we infer the type as `TestEnum | Unknown` without the annotation. We plan to make this configurable in the future and possibly change the default; see #1240 for that.

---

_Closed by @AlexWaygood on 2026-01-06 21:10_

---
