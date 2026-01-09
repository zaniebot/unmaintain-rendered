---
number: 11865
title: "When `TOML parse error` occurred, line numbers with error was incorrectly."
type: issue
state: closed
author: yuji38kwmt
labels:
  - bug
assignees: []
created_at: 2024-06-13T20:26:04Z
updated_at: 2024-06-14T05:03:03Z
url: https://github.com/astral-sh/ruff/issues/11865
synced_at: 2026-01-07T13:12:15-06:00
---

# When `TOML parse error` occurred, line numbers with error was incorrectly.

---

_Issue opened by @yuji38kwmt on 2024-06-13 20:26_

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


# Environments

### Ruff version
```
$ ruff --version
ruff 0.4.8
```
### Directory structure
```
$ tree
.
├── README.md
├── ruff.toml
├── src
│   └── __init__.py
└── tests
    └── __init__.py

2 directories, 4 files
```

### `ruff.toml`

```toml
target-version = "py311"

# invalid
[tool.ruff.lint]
ignore = [
    "G004"
]
```

# What happened?
I executed `ruff check` command. But `TOML parse error` occurred.

```
$ ruff check src
ruff failed
  Cause: Failed to parse /home/yuji/tmp/20240613/ruff.toml
  Cause: TOML parse error at line 1, column 1
  |
1 | target-version = "py311"
  | ^^^^^^^^^^^^^^^^^^^^^^^^
unknown field `tool`
```

But the 1st line `target-version = "py311"` is valid.
The 4th line `[tool.ruff.lint]` is invalid.

# What to expect?
When `TOML parse error` occurred, I want the line numbers with error to be displayed correctly.




---

_Comment by @charliermarsh on 2024-06-13 23:55_

That's strange. I see the same thing, although I don't _think_ we have any control over this.

---

_Label `bug` added by @charliermarsh on 2024-06-13 23:55_

---

_Comment by @zanieb on 2024-06-14 00:32_

I think this is a duplicate of https://github.com/astral-sh/ruff/issues/9719

---

_Comment by @yuji38kwmt on 2024-06-14 02:13_

>although I don't think we have any control over this.

Thanks!  
I see, I understand.

---

_Closed by @MichaReiser on 2024-06-14 05:03_

---
