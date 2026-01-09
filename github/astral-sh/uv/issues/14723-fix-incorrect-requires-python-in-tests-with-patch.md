---
number: 14723
title: "Fix incorrect `requires-python` in tests with patch versions"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - testing
assignees: []
created_at: 2025-07-18T13:29:27Z
updated_at: 2025-07-20T16:12:02Z
url: https://github.com/astral-sh/uv/issues/14723
synced_at: 2026-01-07T13:12:18-06:00
---

# Fix incorrect `requires-python` in tests with patch versions

---

_Issue opened by @zanieb on 2025-07-18 13:29_

e.g., if a test case has `requires-python = ">=3.13.2"` but the test context only requests `3.13` we will fail with a missing interpreter at test runtime if we happen to select an older interpreter, like `3.13.0`

---

_Label `good first issue` added by @zanieb on 2025-07-18 13:29_

---

_Label `testing` added by @zanieb on 2025-07-18 13:29_

---

_Referenced in [astral-sh/uv#14733](../../astral-sh/uv/pulls/14733.md) on 2025-07-18 16:52_

---

_Closed by @zanieb on 2025-07-20 16:12_

---
