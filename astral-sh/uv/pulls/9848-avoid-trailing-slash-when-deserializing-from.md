```yaml
number: 9848
title: Avoid trailing slash when deserializing from lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/re
created_at: 2024-12-12T18:35:18Z
updated_at: 2024-12-12T18:49:06Z
url: https://github.com/astral-sh/uv/pull/9848
synced_at: 2026-01-12T16:09:00Z
```

# Avoid trailing slash when deserializing from lockfile

---

_@charliermarsh_

## Summary

Very tricky problem whereby `workspace_root.join(path)` returns the workspace root with a trailing slash if `path` is empty... This caused us to accidentally _include_ excluded members during workspace discovery, since (e.g.) `packages/seeds` doesn't match `packages/seeds/`.

Closes https://github.com/astral-sh/uv/issues/9832#issuecomment-2539121761.


---

_Label `bug` added by @charliermarsh on 2024-12-12 18:35_

---

_Merged by @charliermarsh on 2024-12-12 18:49_

---

_Closed by @charliermarsh on 2024-12-12 18:49_

---

_Branch deleted on 2024-12-12 18:49_

---
