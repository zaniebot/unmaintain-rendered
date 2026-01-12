```yaml
number: 13678
title: "Avoid reinstalling dependency group members with `--all-packages`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dev
created_at: 2025-05-27T12:31:38Z
updated_at: 2025-05-27T12:43:08Z
url: https://github.com/astral-sh/uv/pull/13678
synced_at: 2026-01-12T16:10:48Z
```

# Avoid reinstalling dependency group members with `--all-packages`

---

_@charliermarsh_

## Summary

Right now, if a workspace member is first created by way of being a dev dependency on another member, we end up duplicating it in the graph. Instead, we should create all the roots upfront; all subsequent node creations are robust to existing nodes.

Closes https://github.com/astral-sh/uv/issues/13673#issuecomment-2912196406.


---

_Label `bug` added by @charliermarsh on 2025-05-27 12:31_

---

_Merged by @charliermarsh on 2025-05-27 12:43_

---

_Closed by @charliermarsh on 2025-05-27 12:43_

---

_Branch deleted on 2025-05-27 12:43_

---
