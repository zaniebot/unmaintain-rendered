```yaml
number: 1524
title: Ignoring certain checks in a certain directory or files named a certain way
type: issue
state: closed
author: not-my-profile
labels: []
assignees: []
created_at: 2023-01-01T04:47:37Z
updated_at: 2023-01-01T04:49:32Z
url: https://github.com/astral-sh/ruff/issues/1524
synced_at: 2026-01-10T12:05:30Z
```

# Ignoring certain checks in a certain directory or files named a certain way

---

_Issue opened by @not-my-profile on 2023-01-01 04:47_

You probably want to disable the public function/class/etc. should have a docstring lints for all files in `tests/` or all files named `test_*.py`.

The [per-file-ignores](https://github.com/charliermarsh/ruff#per-file-ignores) setting however apparently does not support specifying directories or specifying wildcards.

---

_Comment by @not-my-profile on 2023-01-01 04:49_

Nevermind wildcards do work ... they're just not documented.

---

_Closed by @not-my-profile on 2023-01-01 04:49_

---
