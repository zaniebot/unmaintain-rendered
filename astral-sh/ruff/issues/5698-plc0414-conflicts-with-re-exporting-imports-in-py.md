```yaml
number: 5698
title: PLC0414 conflicts with re-exporting imports in py files
type: issue
state: closed
author: twoertwein
labels: []
assignees: []
created_at: 2023-07-11T23:39:53Z
updated_at: 2023-07-12T03:09:25Z
url: https://github.com/astral-sh/ruff/issues/5698
synced_at: 2026-01-12T15:54:45Z
```

# PLC0414 conflicts with re-exporting imports in py files

---

_@twoertwein_

Pandas uses similar code in `pandas/_typing.py`

```py
if TYPE_CHECKING:
    if sys.version_info >= (3, 10):
        from typing import TypeGuard
    else:
        from typing_extensions import TypeGuard
else:
    TypeGuard: Any = None
```

`TypeGuard` is not used inside this file (pyright detects this). A fix for pyright is to use the redundant `as TypeGuard` import but PLC0414 conflicts with that. Alternatives are: ignore the pyright error or create `__all__` (but that would need to contain a lot of symbols).

xref https://github.com/astral-sh/ruff/issues/3734 https://github.com/pandas-dev/pandas/pull/54085

---

_Comment by @zanieb on 2023-07-11 23:47_

Thanks for the clear issue!

`PLC0414` is not enabled by default — perhaps you don't want to use it in Pandas if you need to export names with the redundant `as ...` or perhaps it'd make sense to disable it in that file?

What do you see as the ideal behavior for Ruff here?

---

_Comment by @twoertwein on 2023-07-12 00:08_

Thank you, ignoring this rule (for this one file/or all) is probably the best option.

As far as I remember, flake8's F401 (haven't used it since switching to ruff :) ) also treats explicit re-exports as used imports.

---

_Comment by @zanieb on 2023-07-12 00:11_

Yes F401 will be raised on `from . import foo` but not `from . import foo as foo` (some discussion on that right now at #5697)

Anything else you can see Ruff doing better here or should we close this?

---

_Comment by @twoertwein on 2023-07-12 00:13_

Can be closed.

> Anything else you can see Ruff doing better here or should we close this?

(Consider removing this rule.) I will ignore this rule.

---

_Closed by @twoertwein on 2023-07-12 00:13_

---

_Comment by @charliermarsh on 2023-07-12 03:09_

Yeah PLC0414 is a strange rule because explicit re-export is a respected mechanism to indicate publicity by many tools (Ruff, but also Pyright).

---
