```yaml
number: 1880
title: "Type `list[...]` containing negated type is not iterable"
type: issue
state: open
author: mattgiles
labels:
  - bug
assignees: []
created_at: 2025-12-14T04:57:42Z
updated_at: 2025-12-19T15:08:09Z
url: https://github.com/astral-sh/ty/issues/1880
synced_at: 2026-01-12T15:54:26Z
```

# Type `list[...]` containing negated type is not iterable

---

_@mattgiles_

### Summary

I am seeing not-iterable complaints like:

```
Object of type `list[Thing | ~str | Unknown]` is not iterable (not-iterable)
```

As in this [example](https://play.ty.dev/f8cf1369-0e71-4034-bfde-8c718941d6eb).

### Version

ty 0.0.1-alpha.34 (ef3d48ac4 2025-12-12)

---

_Renamed from "Object of type list is not iterable" to "Object of type `list[...]` is not iterable" by @mattgiles on 2025-12-14 05:00_

---

_Comment by @dhruvmanila on 2025-12-15 05:58_

It seems to be related to the fact that there's a negation type in it, this is a minimal example:

```py
from ty_extensions import Not

def foo(value: list[Not[str]]):
    # Object of type `list[~str]` is not iterable (not-iterable)
    for x in value:
		reveal_type(x)  # revealed: ~str
```

The call to `list[~str].__iter__() -> Iterator[~str]` is failing because of

```
Argument type `list[~str]` does not satisfy upper bound `list[_T@list]` of type variable `Self`
```

---

_Label `bug` added by @dhruvmanila on 2025-12-15 06:21_

---

_Renamed from "Object of type `list[...]` is not iterable" to "Type `list[...]` containing negated type is not iterable" by @dhruvmanila on 2025-12-15 06:34_

---

_Added to milestone `Stable` by @carljm on 2025-12-15 19:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-19 15:08_

---
