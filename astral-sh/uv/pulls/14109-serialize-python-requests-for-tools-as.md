```yaml
number: 14109
title: Serialize Python requests for tools as canonicalized strings
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/canonical-python
created_at: 2025-06-17T16:31:05Z
updated_at: 2025-06-17T22:45:13Z
url: https://github.com/astral-sh/uv/pull/14109
synced_at: 2026-01-12T16:11:01Z
```

# Serialize Python requests for tools as canonicalized strings

---

_@zanieb_

When working on support for reading global Python pins in tool operations, I noticed that we weren't using the canonicalized Python request in receipts — we were using the raw string provided by the user. Since we'll need to compare these values, we should be using the canonicalized string.

The `Tool` and `ToolReceipt` types have been updated to hold a `PythonRequest` instead of a `String`, and `Serialize` was implemented for `PythonRequest` so canonicalization can happen at the edge instead of being the caller's responsibility.

---

_Label `bug` added by @zanieb on 2025-06-17 16:31_

---

_Marked ready for review by @zanieb on 2025-06-17 17:26_

---

_@charliermarsh approved on 2025-06-17 20:49_

---

_Merged by @zanieb on 2025-06-17 22:45_

---

_Closed by @zanieb on 2025-06-17 22:45_

---

_Branch deleted on 2025-06-17 22:45_

---
