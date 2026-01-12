```yaml
number: 7785
title: "False `RUF012` on empty mutable collections as default values in `msgspec.Struct`s"
type: issue
state: closed
author: floxay
labels:
  - bug
assignees: []
created_at: 2023-10-03T16:01:02Z
updated_at: 2023-10-03T16:51:27Z
url: https://github.com/astral-sh/ruff/issues/7785
synced_at: 2026-01-12T15:54:47Z
```

# False `RUF012` on empty mutable collections as default values in `msgspec.Struct`s

---

_@floxay_

`msgspec.Structs` allow empty mutable collections as default values, however Ruff currently warns about these.\
[Docs](https://jcristharif.com/msgspec/structs.html#default-values):
> Builtin empty mutable collections ([], {}, set(), and bytearray()) may be used as default values (as in c above). Since defaults of these types are so common, these are “syntactic sugar” for specifying the corresponding default_factory (to avoid accidental sharing of mutable values). A default of [] is identical to a default of field(default_factory=list), with a new list instance used each time.

Ruff version: `0.0.292`

MRE:
```py
import msgspec

class Example(msgspec.Struct):
    c: list[int] = []  # Mutable class attributes should be annotated with `typing.ClassVar` Ruff(RUF012)
```


---

_Comment by @charliermarsh on 2023-10-03 16:02_

Thanks, will fix.

---

_Label `bug` added by @charliermarsh on 2023-10-03 16:02_

---

_Closed by @charliermarsh on 2023-10-03 16:51_

---
