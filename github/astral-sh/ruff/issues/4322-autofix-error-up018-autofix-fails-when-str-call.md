---
number: 4322
title: "[Autofix error] UP018 autofix fails when str() call is inside f-string: `f\"{str()}\"`"
type: issue
state: closed
author: konstin
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-05-09T18:48:08Z
updated_at: 2023-05-18T14:33:34Z
url: https://github.com/astral-sh/ruff/issues/4322
synced_at: 2026-01-07T13:12:14-06:00
---

# [Autofix error] UP018 autofix fails when str() call is inside f-string: `f"{str()}"`

---

_Issue opened by @konstin on 2023-05-09 18:48_

In ruff https://github.com/charliermarsh/ruff/commit/99a755f936ebd73cdd1bfccbc121c31d4b27751e, the following snippet causes the autofix UP018 from the pyupgrade rules to introduce a syntax error:

```python
f"{str()}"
```

```
$ ./ruff --no-cache --select UP018 --fix code.py

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `code.py`, the rule codes UP018, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

code.py:1:4: UP018 Unnecessary call to `str`
Found 1 error.
```

This was originally discovered in https://github.com/Scille/parsec-cloud/blob/114c80f2349e555ebac1aef132ac9289de7c8dff/misc/releaser.py#L120


---

_Label `bug` added by @konstin on 2023-05-09 18:48_

---

_Label `autofix` added by @konstin on 2023-05-09 18:48_

---

_Label `autofix-error` added by @konstin on 2023-05-09 18:48_

---

_Referenced in [astral-sh/ruff#4487](../../astral-sh/ruff/pulls/4487.md) on 2023-05-18 02:47_

---

_Closed by @charliermarsh on 2023-05-18 14:33_

---
