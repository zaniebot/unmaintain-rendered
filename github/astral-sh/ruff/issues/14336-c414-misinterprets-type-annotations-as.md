---
number: 14336
title: C414 Misinterprets Type Annotations as Unnecessary Nesting
type: issue
state: closed
author: laipz8200
labels: []
assignees: []
created_at: 2024-11-14T12:01:48Z
updated_at: 2024-11-14T12:23:21Z
url: https://github.com/astral-sh/ruff/issues/14336
synced_at: 2026-01-07T13:12:16-06:00
---

# C414 Misinterprets Type Annotations as Unnecessary Nesting

---

_Issue opened by @laipz8200 on 2024-11-14 12:01_

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

It appears that C414 is incorrectly flagging certain type annotations as unnecessary nested structures when they are, in fact, valid and necessary for the code. This false positive may cause confusion, as valid type hints are mistakenly categorized as problematic.

<img width="930" alt="image" src="https://github.com/user-attachments/assets/90568dc9-96bf-4048-97e0-f405ed193b99">



---

_Comment by @germaniuss on 2024-11-14 12:21_

I'm not part of the dev team but just an observer, but is seems to me that what you want do to is use brackets instead of parenthesis in your type hint.

---

_Comment by @laipz8200 on 2024-11-14 12:23_

> I'm not part of the dev team but just an observer, but is seems to me that what you want do to is use brackets instead of parenthesis in your type hint.

You're right, it's my miss, sorry.

---

_Closed by @laipz8200 on 2024-11-14 12:23_

---
