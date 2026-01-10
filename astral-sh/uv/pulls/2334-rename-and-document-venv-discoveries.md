```yaml
number: 2334
title: Rename and document venv discoveries
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/document-uv-virtualenv
created_at: 2024-03-10T13:16:45Z
updated_at: 2024-03-10T13:44:51Z
url: https://github.com/astral-sh/uv/pull/2334
synced_at: 2026-01-10T14:54:43Z
```

# Rename and document venv discoveries

---

_Pull request opened by @konstin on 2024-03-10 13:16_

Preparing for #2058, i found it hard to follow where which discovery function gets called. I moved all the discovery functions to a `find_python` module (some exposed through `PythonEnvironment`) and documented which subcommand uses which python discovery strategy.

No functional changes.

![new uv-virtualenv docs page](https://github.com/astral-sh/uv/assets/6826232/cd56df8a-754d-4640-9e7a-e1f9baf6441c)


---

_Label `internal` added by @konstin on 2024-03-10 13:16_

---

_Merged by @konstin on 2024-03-10 13:44_

---

_Closed by @konstin on 2024-03-10 13:44_

---

_Branch deleted on 2024-03-10 13:44_

---
