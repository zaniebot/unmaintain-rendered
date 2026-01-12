```yaml
number: 12421
title: "UP031: fix for `\"%.2X\" % 1` generates broken code"
type: issue
state: closed
author: radoering
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-07-20T16:26:36Z
updated_at: 2024-07-26T15:48:20Z
url: https://github.com/astral-sh/ruff/issues/12421
synced_at: 2026-01-12T15:54:51Z
```

# UP031: fix for `"%.2X" % 1` generates broken code

---

_@radoering_

I know fixing UP031 is unsafe, but in this case we can do better:

`"%.2X" % 1` is converted to `"{:.2X}".format(1)` but should probably be converted to `"{:02X}".format(1)`

```
>>> "%.2X" % 1
'01'
>>> "%02X" % 1
'01'
>>> "{:02X}".format(1)
'01'
>>> "{:.2X}".format(1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: Precision not allowed in integer format specifier
```

Command: `ruff check --select UP031 --fix --unsafe-fixes test.py`
Ruff Version: 0.5.3

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `bug` added by @charliermarsh on 2024-07-20 16:27_

---

_Label `fixes` added by @charliermarsh on 2024-07-20 16:27_

---

_Comment by @charliermarsh on 2024-07-20 16:28_

Agreed.

---

_Comment by @charliermarsh on 2024-07-25 21:35_

So basically: use `:0` rather than `.` for integer-only presentation types.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-26 14:39_

---

_Closed by @charliermarsh on 2024-07-26 15:48_

---
