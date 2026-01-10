```yaml
number: 13492
title: Recode TRY302 to TRY203
type: issue
state: closed
author: wwuck
labels:
  - rule
assignees: []
created_at: 2024-09-24T05:22:48Z
updated_at: 2024-10-08T16:41:36Z
url: https://github.com/astral-sh/ruff/issues/13492
synced_at: 2026-01-10T11:09:55Z
```

# Recode TRY302 to TRY203

---

_Issue opened by @wwuck on 2024-09-24 05:22_

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
tryceratops changed the error code of TRY302 to TRY203.

https://github.com/guilatrova/tryceratops/commit/8826ded502e230aee4c0cfe92c709feb9f431a19

If it's too much bother to change in ruff, can we add some documentation that mentions the new error code in tryceratops in case anyone migrating to ruff misses it?

---

_Renamed from "Wrong error code for TRY302" to "Recode TRY302 to TRY203" by @MichaReiser on 2024-09-24 09:53_

---

_Added to milestone `v0.7` by @MichaReiser on 2024-09-24 09:53_

---

_Label `rule` added by @MichaReiser on 2024-09-24 09:53_

---

_Closed by @AlexWaygood on 2024-10-08 16:41_

---
