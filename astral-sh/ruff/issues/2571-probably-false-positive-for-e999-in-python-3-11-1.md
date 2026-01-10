---
number: 2571
title: Probably false-positive for E999 in Python 3.11.1
type: issue
state: closed
author: divaltor
labels:
  - bug
assignees: []
created_at: 2023-02-04T19:57:51Z
updated_at: 2023-02-21T18:42:23Z
url: https://github.com/astral-sh/ruff/issues/2571
synced_at: 2026-01-10T01:22:41Z
---

# Probably false-positive for E999 in Python 3.11.1

---

_Issue opened by @divaltor on 2023-02-04 19:57_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hello, in Python 3.11.1 with enabled E999 rule ruff throw error on code with catching new exception groups

```python
try:
   ...
except* ValueError as ex:
   ...
```

```bash
$ ruff .
> E999 SyntaxError: invalid syntax. Got unexpected token '*'
```

In settings is set `target-version = "py311"`

---

_Label `bug` added by @charliermarsh on 2023-02-04 20:03_

---

_Comment by @charliermarsh on 2023-02-04 20:03_

Yeah good catch. We'll need to fix this upstream in RustPython.

---

_Comment by @youknowone on 2023-02-10 20:24_

https://github.com/RustPython/RustPython/issues/4479

---

_Referenced in [astral-sh/ruff#3089](../../astral-sh/ruff/pulls/3089.md) on 2023-02-21 18:04_

---

_Closed by @charliermarsh on 2023-02-21 18:42_

---
