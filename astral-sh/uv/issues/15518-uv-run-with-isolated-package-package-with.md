---
number: 15518
title: "`uv run` with `--isolated --package [package] --with [otherpackage]` leaks workspace virtualenv"
type: issue
state: closed
author: bryab
labels:
  - bug
assignees: []
created_at: 2025-08-25T16:13:46Z
updated_at: 2025-08-26T00:24:25Z
url: https://github.com/astral-sh/uv/issues/15518
synced_at: 2026-01-10T01:25:57Z
---

# `uv run` with `--isolated --package [package] --with [otherpackage]` leaks workspace virtualenv

---

_Issue opened by @bryab on 2025-08-25 16:13_

### Summary

UV version: 0.8.7 to Current (0.8.13)

This issue relates to using a UV workspace.  When running with the `--isolated` flag in combination with `--package [workspace member]` and `--with [any other package]`, the project virtualenv is leaked into the "isolated" uv run environment.  If running without the `--with` command, the `uv run` environment is correctly isolated.  This problem was introduced in 0.8.7.

For example if your main workspace `pyproject.toml` has a dependency (e.g. `requests`) and you have a workspace member called `mypackage1` with no dependencies:

First run `uv sync` so that `requests` is installed in the workspace virtualenv.

Without `--with`, the command works as expected, with `requests` not present because it's not a dependency of `mypackage1`:

`uv run --isolated --package mypackage1 python -c "import requests"`

```
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'requests'
```

Now add `--with` with any arbitrary package:

`uv run --isolated --package mypackage1 --with six python -c "import requests"`

The command will run successfully, indicating that requests was successfully imported.

Run the same command in `uv 0.8.6` and it fails as expected:

```
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'requests'
```





### Platform

Darwin 23.6.0 arm64

### Version

uv 0.8.13 (ede75fe62 2025-08-21)

### Python version

Python 3.11.11

---

_Label `bug` added by @bryab on 2025-08-25 16:13_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-25 23:29_

---

_Referenced in [astral-sh/uv#15522](../../astral-sh/uv/pulls/15522.md) on 2025-08-25 23:50_

---

_Closed by @charliermarsh on 2025-08-26 00:24_

---
