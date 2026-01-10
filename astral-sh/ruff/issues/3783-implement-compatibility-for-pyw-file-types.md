```yaml
number: 3783
title: "Implement compatibility for `.pyw` file types"
type: issue
state: closed
author: danparizher
labels:
  - configuration
assignees: []
created_at: 2023-03-28T20:10:27Z
updated_at: 2023-04-12T03:39:46Z
url: https://github.com/astral-sh/ruff/issues/3783
synced_at: 2026-01-10T11:09:46Z
```

# Implement compatibility for `.pyw` file types

---

_Issue opened by @danparizher on 2023-03-28 20:10_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Right now, Ruff CLI commands do not work for `.pyw` files. The syntax of these files are identical to that of `.py` files; they are just used for GUIs.

---

_Comment by @charliermarsh on 2023-03-28 20:28_

This would be solved by #3410.

---

_Renamed from "Implement functionality for `.pyw` file types" to "Implement compatibility for `.pyw` file types" by @danparizher on 2023-03-28 23:00_

---

_Label `configuration` added by @charliermarsh on 2023-03-28 23:32_

---

_Closed by @charliermarsh on 2023-04-12 03:39_

---
