---
number: 9549
title: "FURB118: Failed to create fix for ReimplementedOperator"
type: issue
state: closed
author: 93578237
labels:
  - bug
assignees: []
created_at: 2024-01-16T11:09:40Z
updated_at: 2024-01-16T20:26:19Z
url: https://github.com/astral-sh/ruff/issues/9549
synced_at: 2026-01-07T13:12:15-06:00
---

# FURB118: Failed to create fix for ReimplementedOperator

---

_Issue opened by @93578237 on 2024-01-16 11:09_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```
error: Failed to create fix for ReimplementedOperator: No fix available
```
Code:
```python
from django.template.library import Library

register = Library()


@register.filter  # noqa: FURB118
def mult(val, arg):
    return val * arg
```
Failing with:
```shell
ruff --select FURB ta.py --isolated --preview --show-fixes --no-cache
```
Version: `0.1.13`

Its also failing without noqa

---

_Label `bug` added by @charliermarsh on 2024-01-16 19:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-16 20:15_

---

_Referenced in [astral-sh/ruff#9556](../../astral-sh/ruff/pulls/9556.md) on 2024-01-16 20:21_

---

_Comment by @charliermarsh on 2024-01-16 20:21_

Thanks, fixed in the next release.

---

_Closed by @charliermarsh on 2024-01-16 20:26_

---
