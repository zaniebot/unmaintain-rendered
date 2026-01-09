---
number: 11501
title: Make Bandit suspicious import rules into a single configurable rule
type: issue
state: open
author: inoa-jboliveira
labels:
  - question
assignees: []
created_at: 2024-05-23T00:01:39Z
updated_at: 2024-05-23T02:06:02Z
url: https://github.com/astral-sh/ruff/issues/11501
synced_at: 2026-01-07T13:12:15-06:00
---

# Make Bandit suspicious import rules into a single configurable rule

---

_Issue opened by @inoa-jboliveira on 2024-05-23 00:01_

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

Remove all rules from S401 to S415 (suspicious imports) and create a single S400 rule that says

`"Possibly insecure import {import_name}: {reason}"`

Allow the imports being configured via command line
```
[tool.ruff.lint.flake8_bandit]
suspicious_import = {"marshal" = "Deserialization is possibly dangerous"}

suspicious_import_include = {foobar = "I dont like this one"}

suspicious_import_exclude = [
  subprocess
]
```

To be honest, I just dont think these rules should exist since we have the TID253 configurable via `banned-module-level-imports`.

My issue is with these rules existing and not being in a good range that they can be ignored in bulk. I don't know if on next version we will get S416 or S420 (another range -- maybe I need to ignore the whole S4?) with more of these "unsafe" modules. In theory there could be thousands of unsafe modules and having multiple rules is just bad IMO. 

---

_Renamed from "Make Bandit import rules into a single configurable rule" to "Make Bandit suspicious import rules into a single configurable rule" by @inoa-jboliveira on 2024-05-23 00:01_

---

_Comment by @augustelalande on 2024-05-23 00:15_

If your goal is just to ignore them, you should be good to just ignore S4

---

_Comment by @charliermarsh on 2024-05-23 02:05_

Yeah, `S4` is exactly the "suspicious" import rules.

---

_Label `question` added by @charliermarsh on 2024-05-23 02:06_

---
