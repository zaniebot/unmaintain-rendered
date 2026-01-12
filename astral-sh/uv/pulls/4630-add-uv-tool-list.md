```yaml
number: 4630
title: "Add `uv tool list`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/tool-list
created_at: 2024-06-28T17:06:47Z
updated_at: 2024-06-29T16:35:10Z
url: https://github.com/astral-sh/uv/pull/4630
synced_at: 2026-01-12T16:06:21Z
```

# Add `uv tool list`

---

_@zanieb_

Closes #4486 

What it says on the tin.

We skip tools with malformed receipts now and warn instead of failing all tool operations.

---

_Label `cli` added by @zanieb on 2024-06-28 17:06_

---

_Label `preview` added by @zanieb on 2024-06-28 17:06_

---

_@charliermarsh reviewed on 2024-06-28 18:44_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/list.rs`:31 on 2024-06-28 18:44_

Should we include the version, like `pip list`?

---

_@charliermarsh approved on 2024-06-28 18:44_

---

_@zanieb reviewed on 2024-06-28 18:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:31 on 2024-06-28 18:47_

That seems nice!

I'll probably do in a follow-up to separate it from all this boilerplate. 

---

_Merged by @zanieb on 2024-06-28 22:00_

---

_Closed by @zanieb on 2024-06-28 22:00_

---

_Branch deleted on 2024-06-28 22:00_

---
