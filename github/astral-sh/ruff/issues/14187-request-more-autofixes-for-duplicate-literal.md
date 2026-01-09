---
number: 14187
title: "Request: More autofixes for `duplicate-literal-member`/`PYI062`"
type: issue
state: closed
author: Avasam
labels:
  - fixes
assignees: []
created_at: 2024-11-08T01:35:43Z
updated_at: 2024-11-08T03:45:21Z
url: https://github.com/astral-sh/ruff/issues/14187
synced_at: 2026-01-07T13:12:16-06:00
---

# Request: More autofixes for `duplicate-literal-member`/`PYI062`

---

_Issue opened by @Avasam on 2024-11-08 01:35_

* A minimal code snippet that reproduces

https://docs.astral.sh/ruff/rules/duplicate-literal-member/#duplicate-literal-member-pyi062 mentions that
> Fix is sometimes available.

However cases like this which *looks* trivial is detected but not autofixed.
```py
def foo(bar: Literal["a", "b", "b"]): ...
```

* The current Ruff version (`ruff --version`).
ruff 0.7.2

---

_Referenced in [microsoft/python-type-stubs#340](../../microsoft/python-type-stubs/pulls/340.md) on 2024-11-08 03:11_

---

_Comment by @charliermarsh on 2024-11-08 03:20_

I actually don't think fix is ever available here; it's incorrectly marked as fixable, but we never create fixes for it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-08 03:34_

---

_Label `fixes` added by @charliermarsh on 2024-11-08 03:35_

---

_Referenced in [astral-sh/ruff#14188](../../astral-sh/ruff/pulls/14188.md) on 2024-11-08 03:35_

---

_Closed by @charliermarsh on 2024-11-08 03:45_

---
