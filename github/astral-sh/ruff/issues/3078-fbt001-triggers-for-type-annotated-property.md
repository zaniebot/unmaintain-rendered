---
number: 3078
title: FBT001 triggers for type annotated property setters
type: issue
state: closed
author: jakob-keller
labels:
  - bug
assignees: []
created_at: 2023-02-21T00:00:29Z
updated_at: 2023-02-21T19:31:02Z
url: https://github.com/astral-sh/ruff/issues/3078
synced_at: 2026-01-07T13:12:14-06:00
---

# FBT001 triggers for type annotated property setters

---

_Issue opened by @jakob-keller on 2023-02-21 00:00_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Another edge-case that triggers FBT001, but shouldn't IMO:

```
class Test:
    def __init__(self):
        self._value: bool = False

    @property
    def value(self) -> bool:
        return self._value

    @value.setter
    def value(self, value: bool) -> None:  # triggers FBT001
        self._value = value
```

Obviously, this requires for "FBT" to be included in the select list in settings.

ruff 0.0.249

---

_Label `bug` added by @charliermarsh on 2023-02-21 00:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-21 00:42_

---

_Referenced in [astral-sh/ruff#3092](../../astral-sh/ruff/pulls/3092.md) on 2023-02-21 19:27_

---

_Closed by @charliermarsh on 2023-02-21 19:31_

---
