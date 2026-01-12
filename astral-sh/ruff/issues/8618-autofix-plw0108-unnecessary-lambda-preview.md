```yaml
number: 8618
title: "Autofix `PLW0108` unnecessary-lambda (preview)"
type: issue
state: closed
author: Skylion007
labels:
  - fixes
assignees: []
created_at: 2023-11-11T15:29:01Z
updated_at: 2023-11-11T23:34:03Z
url: https://github.com/astral-sh/ruff/issues/8618
synced_at: 2026-01-12T15:54:48Z
```

# Autofix `PLW0108` unnecessary-lambda (preview)

---

_@Skylion007_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
It would be great to add an autofix to https://github.com/astral-sh/ruff/pull/7953 as it's fix is pretty straightforward (just deleting the lambda and calling the function / method directly). The logic / autofix is somewhat similar to PIE807 which already has an autofix.


---

_Renamed from "Autofix W0108 unnecessary-lambda (preview)" to "Autofix `PLW0108` unnecessary-lambda (preview)" by @Skylion007 on 2023-11-11 15:30_

---

_Comment by @Skylion007 on 2023-11-11 15:32_

At the very least, the cases where the lambda takes no arguments should be trivial to fix.

---

_Label `autofix` added by @charliermarsh on 2023-11-11 23:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-11 23:17_

---

_Closed by @charliermarsh on 2023-11-11 23:34_

---
