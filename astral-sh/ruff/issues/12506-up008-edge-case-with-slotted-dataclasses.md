```yaml
number: 12506
title: UP008 edge-case with slotted dataclasses
type: issue
state: closed
author: alexandermalyga
labels:
  - bug
assignees: []
created_at: 2024-07-25T12:43:15Z
updated_at: 2024-07-26T14:09:52Z
url: https://github.com/astral-sh/ruff/issues/12506
synced_at: 2026-01-10T11:09:54Z
```

# UP008 edge-case with slotted dataclasses

---

_Issue opened by @alexandermalyga on 2024-07-25 12:43_

When using dataclasses with `slots=True`, calling a no-arg `super()` raises a `TypeError`, the workaround of using the two-arg variant triggers a `UP008` rule error. Would be great if the rule took this edge-case into consideration.

From the [docs](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass):

> Calling no-arg super() in dataclasses using slots=True will result in the following exception being raised: TypeError: super(type, obj): obj must be an instance or subtype of type. The two-arg super() is a valid workaround.

How to reproduce:
```python
@dataclass(slots=True)
class Foo:
    bar: str

    def __post_init__(self) -> None:
        self.bar = "foo"


@dataclass(slots=True)
class Baz(Foo):
    def __post_init__(self) -> None:
        super(Baz, self).__post_init__()  # UP008 Use `super()` instead of `super(__class__, self)`
        self.bar = "baz"
```




---

_Renamed from "UP008 with slotted dataclasses" to "UP008 edge-case with slotted dataclasses" by @alexandermalyga on 2024-07-25 12:52_

---

_Label `bug` added by @charliermarsh on 2024-07-25 13:16_

---

_Comment by @charliermarsh on 2024-07-25 13:16_

Wow, never seen that! Thanks, we’ll fix it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-26 13:54_

---

_Closed by @charliermarsh on 2024-07-26 14:09_

---

_Closed by @charliermarsh on 2024-07-26 14:09_

---
