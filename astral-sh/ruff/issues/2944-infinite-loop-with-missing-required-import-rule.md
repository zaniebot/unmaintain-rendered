```yaml
number: 2944
title: "[Infinite loop] with missing required import rule"
type: issue
state: closed
author: underyx
labels:
  - bug
assignees: []
created_at: 2023-02-16T01:25:12Z
updated_at: 2023-02-16T03:24:57Z
url: https://github.com/astral-sh/ruff/issues/2944
synced_at: 2026-01-12T15:54:43Z
```

# [Infinite loop] with missing required import rule

---

_@underyx_

- target file contents before ruff: https://gist.github.com/underyx/ad1478d388cd61bfcb89e25a5ea3ddde
  - after ruff: https://gist.github.com/underyx/5741723fd6ffd373ec96f7967552c123 
  - re-running ruff keeps reporting `100 fixed` even on the 'after ruff' file
- ruff.toml contents: https://gist.github.com/underyx/c0b8d0b6d9c4dd6d1be9447185eaebab
- ruff 0.0.247

```terminal
$ ruff --version
ruff 0.0.247
$ ruff src/semgrep/app/version.py --select I002 --fix --no-cache

error: Failed to converge after 100 iterations.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `src/semgrep/app/version.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

src/semgrep/app/version.py:1:1: I002 Missing required import: `from __future__ import annotations`
Found 101 errors (100 fixed, 1 remaining).
```

---

_Comment by @underyx on 2023-02-16 01:27_

Seems like this simpler target also reproduces the issue:

```py
"""a
b"""
# b
import os
```

---

_Label `bug` added by @charliermarsh on 2023-02-16 01:32_

---

_Comment by @charliermarsh on 2023-02-16 01:32_

Thanks for filing :)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-16 02:17_

---

_Comment by @charliermarsh on 2023-02-16 03:13_

Woah not sure what happened here but definitely broken! Will make sure it's fixed for the next release (maybe Friday-ish).

---

_Closed by @charliermarsh on 2023-02-16 03:24_

---
