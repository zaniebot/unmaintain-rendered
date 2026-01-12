```yaml
number: 15153
title: "Fix regression where `--require-hashes` applied to build dependencies in `uv pip install`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/build-hasher
created_at: 2025-08-07T21:30:11Z
updated_at: 2025-08-07T21:43:25Z
url: https://github.com/astral-sh/uv/pull/15153
synced_at: 2026-01-12T16:11:36Z
```

# Fix regression where `--require-hashes` applied to build dependencies in `uv pip install`

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/15146


---

_Label `bug` added by @zanieb on 2025-08-07 21:30_

---

_@zanieb reviewed on 2025-08-07 21:33_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/install.rs`:585 on 2025-08-07 21:33_

Kind of terrifying this mistake is so easy to make

---

_@hauntsaninja approved on 2025-08-07 21:33_

---

_Merged by @zanieb on 2025-08-07 21:43_

---

_Closed by @zanieb on 2025-08-07 21:43_

---

_Branch deleted on 2025-08-07 21:43_

---
