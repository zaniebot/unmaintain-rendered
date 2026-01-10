```yaml
number: 9427
title: Allow syncing to empty virtual environment directories
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/env-empty
created_at: 2024-11-25T20:47:38Z
updated_at: 2024-11-25T22:23:52Z
url: https://github.com/astral-sh/uv/pull/9427
synced_at: 2026-01-10T12:00:00Z
```

# Allow syncing to empty virtual environment directories

---

_Pull request opened by @zanieb on 2024-11-25 20:47_

As discussed in https://github.com/astral-sh/uv/issues/9423, it's confusing that we do not allow `uv sync` just because the `.venv` directory _exists_. This change matches `uv venv`.

---

_Label `bug` added by @zanieb on 2024-11-25 20:47_

---

_Marked ready for review by @zanieb on 2024-11-25 20:50_

---

_@charliermarsh approved on 2024-11-25 21:56_

---

_Merged by @zanieb on 2024-11-25 22:23_

---

_Closed by @zanieb on 2024-11-25 22:23_

---

_Branch deleted on 2024-11-25 22:23_

---
