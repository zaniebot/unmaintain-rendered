```yaml
number: 1163
title: Validation of attribute assignments does not handle unions/intersections correctly
type: issue
state: open
author: Glyphack
labels:
  - bug
  - attribute access
assignees: []
created_at: 2025-09-10T13:09:49Z
updated_at: 2025-12-02T03:23:09Z
url: https://github.com/astral-sh/ty/issues/1163
synced_at: 2026-01-10T01:58:59Z
```

# Validation of attribute assignments does not handle unions/intersections correctly

---

_Issue opened by @Glyphack on 2025-09-10 13:09_

### Summary

This example is extracted from mdtests after adding explicit `self` annotation (related https://github.com/astral-sh/ruff/pull/18007)

After adding self annotation the following example fails:

```py
from typing import Literal, Any

class DataDescriptor:
    def __get__(self: "DataDescriptor", instance: object, owner: type | None = None) -> Literal["data"]:
        return "data"

    def __set__(self: "DataDescriptor", instance: object, value: int) -> None:
        pass

def _(flag: bool):
    class Meta7(type):
        if flag:
            attr: DataDescriptor = DataDescriptor()
        else:
            attr: Literal[2] = 2

    class C7(metaclass=Meta7):
        pass

    # Invalid assignment to data descriptor attribute `attr` on type `<class 'C7'>` with custom `__set__` method (invalid-assignment)
    C7.attr = 2 if flag else 100
```
https://play.ty.dev/d25588da-713f-44d3-b35e-937808f43c06

I expected the error to be related to DataDescriptor, since we added the `self` annotation and it failed.
I could not find why adding `self` annotation causes the issue so I tried to change the other annotation.

This example has no diagnostics:

```py
from typing import Literal, Any

class DataDescriptor:
    def __get__(self: "DataDescriptor", instance: object, owner: type | None = None) -> Literal["data"]:
        return "data"

    def __set__(self: "DataDescriptor", instance: object, value: int) -> None:
        pass

def _(flag: bool):
    class Meta7(type):
        attr: DataDescriptor = DataDescriptor()

    class C7(metaclass=Meta7):
        pass

    C7.attr = 2 if flag else 100
```

### Version

fd7eb1e22

---

_Renamed from "False positive `invalid-assignment` for union of `DataDescriptor` and `Literal` in metaclass attribute" to "False positive `invalid-assignment` on `DataDescriptor` metaclass attribute when using explicit `self` annotation" by @Glyphack on 2025-09-10 13:12_

---

_Comment by @carljm on 2025-09-10 18:04_

@sharkdp Is this something you can look into?

---

_Assigned to @sharkdp by @sharkdp on 2025-09-10 18:25_

---

_Comment by @sharkdp on 2025-09-10 18:26_

Yes, I noticed this too on the test changes for https://github.com/astral-sh/ruff/pull/20303, and wanted to look into it anyway. Thanks for reporting this separately @Glyphack.

---

_Renamed from "False positive `invalid-assignment` on `DataDescriptor` metaclass attribute when using explicit `self` annotation" to "Validation of attribute assignments does not handle unions/intersections correctly" by @sharkdp on 2025-09-16 13:19_

---

_Label `bug` added by @sharkdp on 2025-09-16 13:19_

---

_Label `attribute access` added by @sharkdp on 2025-09-16 13:19_

---

_Comment by @sharkdp on 2025-09-16 13:44_

The core issue here is that `validate_attribute_assignment` doesn't handle unions/intersections of attributes correctly. This can also be demonstrated with a much smaller, and easier-to-understand example. An error should be emitted in the final line here, but currently isn't:
```py
def _(flag: bool):
    class C:
        if flag:
            attr: int = 1
        else:
            attr: str = ""

    C.attr = 2  # should be an error
```
https://play.ty.dev/f0d21117-4aa3-4ff4-9546-020bfcbfd84d

In the OP example, this is a related problem, where we look up the type of the attribute on the meta type, and get `DataDescriptor | Literal[2]` as a response. Instead of checking assignability to both of these types, we treat them as a union and try to call `__set__`. This leads to a problem, because `DataDescriptor | Literal[2]` is not assignable to the now-annotated `self` attribute on the `__set__` method.

The solution to this is to properly distribute our assignment validation logic over unions and intersections.

It doesn't seem like a high-priority issue for me, since it never came up elsewhere, and since conflicting declarations like these are not supported by any other type checker. I propose to keep this open for now, and fix it after the type-of-self changes have landed, after which it will be easier to see the impact of our fix. In the type-of-self PR, we can add a TODO to the test-case above for now.

FYI @AlexWaygood since you were working on a refactor of `validate_attribute_assignment`?

---

_Comment by @AlexWaygood on 2025-09-16 13:53_

> FYI [@AlexWaygood](https://github.com/AlexWaygood) since you were working on a refactor of `validate_attribute_assignment`?

Still am! It's still quite WIP right now though -- see https://github.com/astral-sh/ruff/pull/19936. And https://github.com/astral-sh/ruff/pull/20368 is higher priority for me to finish. And this week my top priority is working on a couple of release blockers for CPython 3.14.

---

_Unassigned @sharkdp by @sharkdp on 2025-09-30 07:59_

---

_Added to milestone `Stable` by @carljm on 2025-12-02 03:23_

---
