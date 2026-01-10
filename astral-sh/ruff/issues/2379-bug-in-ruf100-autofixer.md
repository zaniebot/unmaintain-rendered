```yaml
number: 2379
title: "Bug in `RUF100` autofixer"
type: issue
state: closed
author: stinodego
labels:
  - bug
assignees: []
created_at: 2023-01-31T05:46:47Z
updated_at: 2023-01-31T12:59:18Z
url: https://github.com/astral-sh/ruff/issues/2379
synced_at: 2026-01-10T11:09:45Z
```

# Bug in `RUF100` autofixer

---

_Issue opened by @stinodego on 2023-01-31 05:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

Using all default settings and `select = ["RUF"]`, the following `noqa` statement is incorrectly fixed.

Before:
```python
"shape:  (6,)\nSeries: '' [duration[μs]]\n[\n\t0µs\n\t1µs\n\t2µs\n\t3µs\n\t4µs\n\t5µs\n]"  # noqa: E501
```

After:
```python
"shape:  (6,)\nSeries: '' [duration[μs]]\n[\n\t0µs\n\t1µs\n\t2µs\n\t3µs\n\t4µs\n\t5µs\n]"  # noq
```

I'm on Ruff version 0.0.237, Python 3.11.0.

---

_Renamed from "Bug in " to "Bug in `RUF100`" by @stinodego on 2023-01-31 05:47_

---

_Renamed from "Bug in `RUF100`" to "Bug in `RUF100` autofixer" by @stinodego on 2023-01-31 05:48_

---

_Label `bug` added by @charliermarsh on 2023-01-31 12:18_

---

_Comment by @charliermarsh on 2023-01-31 12:18_

Thank you for filing!

---

_Comment by @stinodego on 2023-01-31 12:21_

I was surprised, as I hadn't run into a bug yet. Keep up the good work!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-31 12:42_

---

_Closed by @charliermarsh on 2023-01-31 12:59_

---
