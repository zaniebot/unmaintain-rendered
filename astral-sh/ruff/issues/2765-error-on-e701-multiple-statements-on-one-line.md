```yaml
number: 2765
title: Error on E701 Multiple statements on one line (colon)
type: issue
state: closed
author: cclauss
labels:
  - bug
assignees: []
created_at: 2023-02-11T15:14:24Z
updated_at: 2023-02-11T17:22:55Z
url: https://github.com/astral-sh/ruff/issues/2765
synced_at: 2026-01-12T15:54:43Z
```

# Error on E701 Multiple statements on one line (colon)

---

_@cclauss_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Ruff v0.0.245 raises a `Multiple statements on one line (colon)` error while v0.0.244 did not.
`total_color = success_color_fn if all_green else lambda x: x`
`openlibrary/i18n/__init__.py:234:70: E701 Multiple statements on one line (colon)`

---

_Label `bug` added by @charliermarsh on 2023-02-11 17:00_

---

_Comment by @charliermarsh on 2023-02-11 17:00_

Thanks! Will fix.

---

_Closed by @charliermarsh on 2023-02-11 17:22_

---
