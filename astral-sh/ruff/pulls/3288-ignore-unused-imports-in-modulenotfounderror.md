```yaml
number: 3288
title: Ignore unused imports in ModuleNotFoundError blocks
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/module-not-found
created_at: 2023-03-01T04:25:28Z
updated_at: 2023-03-01T23:08:38Z
url: https://github.com/astral-sh/ruff/pull/3288
synced_at: 2026-01-12T15:55:12Z
```

# Ignore unused imports in ModuleNotFoundError blocks

---

_@charliermarsh_

I'm actually not sold on this being a good change, but it would close #3279.

---

_Label `bug` added by @charliermarsh on 2023-03-01 04:25_

---

_Comment by @bluetech on 2023-03-01 20:03_

FWIW, IMO ignoring unused imports inside `ModuleNotFoundError` try-except makes a lot of sense.

There's also a lot of code which uses the less specific `ImportError` for this (maybe because `ModuleNotFoundError` was only added in python 3.6), but they'd be better served by switching to `ModuleNotFoundError` so maybe start without it.

---

_Merged by @charliermarsh on 2023-03-01 23:08_

---

_Closed by @charliermarsh on 2023-03-01 23:08_

---

_Branch deleted on 2023-03-01 23:08_

---
