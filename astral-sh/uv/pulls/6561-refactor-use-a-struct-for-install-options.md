```yaml
number: 6561
title: "refactor: use a struct for install options"
type: pull_request
state: merged
author: mkniewallner
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor/add-install-options-struct
created_at: 2024-08-23T23:30:51Z
updated_at: 2024-08-27T11:13:07Z
url: https://github.com/astral-sh/uv/pull/6561
synced_at: 2026-01-12T16:07:25Z
```

# refactor: use a struct for install options

---

_@mkniewallner_

## Summary

Closes #6545.

## Test Plan

Relying on existing tests.

---

_@zanieb reviewed on 2024-08-23 23:38_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:288 on 2024-08-23 23:38_

Should this be `InstallOptions::filter_resolution(...)` or something? (and the helpers would all become private members?)

---

_Comment by @zanieb on 2024-08-23 23:39_

Awesome thank you!

---

_Marked ready for review by @mkniewallner on 2024-08-24 00:11_

---

_@zanieb approved on 2024-08-27 10:37_

Thank you!

---

_Merged by @zanieb on 2024-08-27 10:38_

---

_Closed by @zanieb on 2024-08-27 10:38_

---

_Label `internal` added by @zanieb on 2024-08-27 10:38_

---

_Branch deleted on 2024-08-27 11:13_

---
