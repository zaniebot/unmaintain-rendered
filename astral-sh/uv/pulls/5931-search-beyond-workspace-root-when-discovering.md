```yaml
number: 5931
title: Search beyond workspace root when discovering configuration
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - configuration
  - preview
assignees: []
merged: true
base: main
head: charlie/work
created_at: 2024-08-08T20:49:16Z
updated_at: 2024-08-08T21:05:04Z
url: https://github.com/astral-sh/uv/pull/5931
synced_at: 2026-01-10T13:31:54Z
```

# Search beyond workspace root when discovering configuration

---

_Pull request opened by @charliermarsh on 2024-08-08 20:49_

## Summary

Previously, we wouldn't respect configuration files in directories _above_ a workspace root. But this is somewhat problematic, because any `pyproject.toml` will define a workspace root...

Instead, I think we should _start_ the search at the workspace root, but go above it if necessary.

Closes: #5929.

See: https://github.com/astral-sh/uv/pull/4295.


---

_Renamed from "Ignore workspaces when discovering configuration" to "Search beyond workspace root when discovering configuration" by @charliermarsh on 2024-08-08 20:49_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-08 20:49_

---

_Label `bug` added by @charliermarsh on 2024-08-08 20:49_

---

_Label `preview` added by @charliermarsh on 2024-08-08 20:49_

---

_Label `configuration` added by @charliermarsh on 2024-08-08 20:49_

---

_Comment by @charliermarsh on 2024-08-08 20:51_

I think if we search above a workspace, though, we may want to consider ignoring `pyproject.toml` and only looking at `uv.toml`.

---

_Comment by @zanieb on 2024-08-08 20:54_

> I think if we search above a workspace, though, we may want to consider ignoring pyproject.toml and only looking at uv.toml.

I think that makes sense

---

_Comment by @charliermarsh on 2024-08-08 21:04_

I'll do it separately.

---

_Merged by @charliermarsh on 2024-08-08 21:05_

---

_Closed by @charliermarsh on 2024-08-08 21:05_

---

_Branch deleted on 2024-08-08 21:05_

---
