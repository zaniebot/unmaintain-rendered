---
number: 13501
title: Add checks from flake8-strftime
type: issue
state: open
author: wwuck
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-09-24T13:41:31Z
updated_at: 2024-09-24T14:00:18Z
url: https://github.com/astral-sh/ruff/issues/13501
synced_at: 2026-01-07T13:12:15-06:00
---

# Add checks from flake8-strftime

---

_Issue opened by @wwuck on 2024-09-24 13:41_

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
https://github.com/python-formate/flake8_strftime

- [ ] SFT001 - Linux-specific strftime code used
- [ ] SFT002 - Windows-specific strftime code used

---

_Label `rule` added by @MichaReiser on 2024-09-24 13:59_

---

_Label `needs-decision` added by @MichaReiser on 2024-09-24 13:59_

---

_Comment by @MichaReiser on 2024-09-24 14:00_

Seems reasonable. But we must decide on a category or wait until after #1774 . A new category for just two rules feels very heavy.

---
