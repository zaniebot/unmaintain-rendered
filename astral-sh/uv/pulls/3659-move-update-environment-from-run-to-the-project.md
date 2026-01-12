```yaml
number: 3659
title: "Move `update_environment` from `run` to the `project` namespace"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/move-update-env
created_at: 2024-05-19T19:35:22Z
updated_at: 2024-05-20T01:48:25Z
url: https://github.com/astral-sh/uv/pull/3659
synced_at: 2026-01-12T16:05:47Z
```

# Move `update_environment` from `run` to the `project` namespace

---

_@zanieb_

Prompted by https://github.com/astral-sh/uv/pull/3657#discussion_r1606041239

There's still some level of discomfort here, as the `tool` module needs needs to import the `project` module to manage an environment. We should probably move most of the basic operations in the `project` module root into some sort of shared module for behind the scenes operations?

Regardless, this change should simplify that future move.

---

_Label `internal` added by @zanieb on 2024-05-19 19:37_

---

_Marked ready for review by @zanieb on 2024-05-19 19:37_

---

_@charliermarsh approved on 2024-05-20 00:37_

---

_Merged by @zanieb on 2024-05-20 01:48_

---

_Closed by @zanieb on 2024-05-20 01:48_

---

_Branch deleted on 2024-05-20 01:48_

---
