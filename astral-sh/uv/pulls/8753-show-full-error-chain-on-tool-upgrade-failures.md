```yaml
number: 8753
title: Show full error chain on tool upgrade failures
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/tool-err-chain
created_at: 2024-11-01T13:36:16Z
updated_at: 2024-11-02T16:55:44Z
url: https://github.com/astral-sh/uv/pull/8753
synced_at: 2026-01-12T16:08:29Z
```

# Show full error chain on tool upgrade failures

---

_@zanieb_

As reported in https://github.com/astral-sh/uv/issues/8555

---

_Label `error messages` added by @zanieb on 2024-11-01 13:36_

---

_@charliermarsh reviewed on 2024-11-02 16:50_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/upgrade.rs`:139 on 2024-11-02 16:50_

Previously we only showed the name if the user provided more than one tool. I don't think it's that important, though.

---

_@charliermarsh approved on 2024-11-02 16:50_

---

_@zanieb reviewed on 2024-11-02 16:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/upgrade.rs`:139 on 2024-11-02 16:55_

Good catch, agree it seems fine to always show the name.

---

_Merged by @zanieb on 2024-11-02 16:55_

---

_Closed by @zanieb on 2024-11-02 16:55_

---

_Branch deleted on 2024-11-02 16:55_

---
