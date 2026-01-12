```yaml
number: 13423
title: "`ruff check --add-noqa` doesn't respect `[tool.ruff.lint]` exclusions"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-09-20T18:56:59Z
updated_at: 2024-09-20T19:39:37Z
url: https://github.com/astral-sh/ruff/issues/13423
synced_at: 2026-01-12T15:54:53Z
```

# `ruff check --add-noqa` doesn't respect `[tool.ruff.lint]` exclusions

---

_@charliermarsh_

It will run over all included files, even if the linter excludes them. As such, `--add-noqa` will modify files that wouldn't be reported on with `ruff check`.

---

_Label `bug` added by @charliermarsh on 2024-09-20 18:57_

---

_Label `help wanted` added by @charliermarsh on 2024-09-20 18:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-20 19:27_

---

_Closed by @charliermarsh on 2024-09-20 19:39_

---
