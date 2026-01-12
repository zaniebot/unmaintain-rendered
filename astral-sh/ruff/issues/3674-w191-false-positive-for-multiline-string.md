```yaml
number: 3674
title: W191 False positive for multiline string
type: issue
state: closed
author: Nodd
labels:
  - bug
assignees: []
created_at: 2023-03-22T20:01:18Z
updated_at: 2023-04-07T02:25:41Z
url: https://github.com/astral-sh/ruff/issues/3674
synced_at: 2026-01-12T15:54:43Z
```

# W191 False positive for multiline string

---

_@Nodd_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Applying ruff on the following file (notice the tab indent in the string), ruff yields a error:
a.py :
```python
a="""
	a
"""
```
This is the command I executed:
```bash
$ ruff --version
ruff 0.0.257
$ ruff a.py --select W191
a.py:2:1: W191 Indentation contains tabs
Found 1 error.
```
Ruff should leave alone the text inside strings, since it could be anything.

---

_Comment by @charliermarsh on 2023-03-22 20:01_

Pycodestyle also behaves this way IIRC, but I agree we should fix it.

---

_Label `bug` added by @charliermarsh on 2023-03-22 20:01_

---

_Comment by @evanrittenhouse on 2023-03-25 14:23_

Duplicate of #3438 - I can pick this up

---

_Assigned to @evanrittenhouse by @charliermarsh on 2023-03-25 15:17_

---

_Comment by @Nodd on 2023-03-25 19:31_

Indeed, sorry for the duplicate!

---

_Comment by @charliermarsh on 2023-03-25 19:41_

All good friend!

---

_Closed by @charliermarsh on 2023-04-07 02:25_

---
