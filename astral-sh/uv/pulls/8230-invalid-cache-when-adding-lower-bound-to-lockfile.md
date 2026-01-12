```yaml
number: 8230
title: Invalid cache when adding lower bound to lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dbg
created_at: 2024-10-15T23:00:21Z
updated_at: 2024-10-15T23:09:56Z
url: https://github.com/astral-sh/uv/pull/8230
synced_at: 2026-01-12T16:08:13Z
```

# Invalid cache when adding lower bound to lockfile

---

_@charliermarsh_

## Summary

This was already properly handled, but the operation itself was in a `debug_assert!`, so it wasn't running at all in production builds...

Closes https://github.com/astral-sh/uv/issues/8208.


---

_Label `bug` added by @charliermarsh on 2024-10-15 23:00_

---

_Merged by @charliermarsh on 2024-10-15 23:09_

---

_Closed by @charliermarsh on 2024-10-15 23:09_

---

_Branch deleted on 2024-10-15 23:09_

---
