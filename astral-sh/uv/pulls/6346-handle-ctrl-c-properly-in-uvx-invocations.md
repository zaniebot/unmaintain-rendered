```yaml
number: 6346
title: "Handle Ctrl-C properly in `uvx` invocations"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/exec
created_at: 2024-08-21T16:01:54Z
updated_at: 2024-08-21T16:39:12Z
url: https://github.com/astral-sh/uv/pull/6346
synced_at: 2026-01-12T16:07:19Z
```

# Handle Ctrl-C properly in `uvx` invocations

---

_@charliermarsh_

## Summary

This follows Rye's approach, and solves https://github.com/astral-sh/uv/issues/6334.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-21 16:01_

---

_Label `bug` added by @charliermarsh on 2024-08-21 16:01_

---

_Renamed from "Use exec spawn for uvx" to "Handle Ctrl-C properly in `uvx` invocations" by @charliermarsh on 2024-08-21 16:02_

---

_@zanieb approved on 2024-08-21 16:06_

---

_Comment by @zanieb on 2024-08-21 16:07_

Did you check if this has performance implications? Presumably a bit faster but curious.

---

_Comment by @charliermarsh on 2024-08-21 16:07_

I can give it a try.

---

_Merged by @charliermarsh on 2024-08-21 16:39_

---

_Closed by @charliermarsh on 2024-08-21 16:39_

---

_Branch deleted on 2024-08-21 16:39_

---
