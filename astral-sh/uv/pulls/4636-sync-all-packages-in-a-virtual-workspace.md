```yaml
number: 4636
title: Sync all packages in a virtual workspace
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/virtual-workspace
created_at: 2024-06-28T20:54:56Z
updated_at: 2024-06-29T16:44:01Z
url: https://github.com/astral-sh/uv/pull/4636
synced_at: 2026-01-12T16:06:21Z
```

# Sync all packages in a virtual workspace

---

_@charliermarsh_

## Summary

This PR dodges some of the bigger issues raised by https://github.com/astral-sh/uv/pull/4554 and https://github.com/astral-sh/uv/pull/4555 by _not_ changing any of the bigger semantics around syncing and instead merely changing virtual workspace roots to sync all packages in the workspace (rather than erroring due to being unable to find a project).

Closes #4541.


---

_Review requested from @zanieb by @charliermarsh on 2024-06-28 20:54_

---

_Review requested from @konstin by @charliermarsh on 2024-06-28 20:55_

---

_Label `bug` added by @charliermarsh on 2024-06-28 20:55_

---

_Label `preview` added by @charliermarsh on 2024-06-28 20:55_

---

_Comment by @charliermarsh on 2024-06-29 16:43_

I'm gonna merge this to get virtual workspaces working (which aren't really usable right now), will follow-up on any post-merge reviews.

---

_Merged by @charliermarsh on 2024-06-29 16:43_

---

_Closed by @charliermarsh on 2024-06-29 16:43_

---

_Branch deleted on 2024-06-29 16:44_

---
