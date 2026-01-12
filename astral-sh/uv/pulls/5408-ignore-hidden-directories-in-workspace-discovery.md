```yaml
number: 5408
title: Ignore hidden directories in workspace discovery
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/hidden
created_at: 2024-07-24T14:24:48Z
updated_at: 2024-07-24T16:21:35Z
url: https://github.com/astral-sh/uv/pull/5408
synced_at: 2026-01-12T16:06:48Z
```

# Ignore hidden directories in workspace discovery

---

_@charliermarsh_

## Summary

This is surprisingly complex because we need to decide what happens if you run `uv run` from within a hidden folder, etc. For now, I did the simplest thing: we just ignore workspace members that are hidden directories if they lack a `pyproject.toml`, so you can still include hidden members, they're just ignored if they don't seem to be projects.

Closes https://github.com/astral-sh/uv/issues/5403.


---

_Review requested from @konstin by @charliermarsh on 2024-07-24 14:24_

---

_Label `bug` added by @charliermarsh on 2024-07-24 14:24_

---

_Label `preview` added by @charliermarsh on 2024-07-24 14:24_

---

_Merged by @charliermarsh on 2024-07-24 16:21_

---

_Closed by @charliermarsh on 2024-07-24 16:21_

---

_Branch deleted on 2024-07-24 16:21_

---
