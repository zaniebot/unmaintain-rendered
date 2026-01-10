---
number: 12872
title: how to set MAX_JOBS for ninja while using uv
type: issue
state: closed
author: DefTruth
labels:
  - question
assignees: []
created_at: 2025-04-14T07:58:07Z
updated_at: 2025-04-14T12:23:22Z
url: https://github.com/astral-sh/uv/issues/12872
synced_at: 2026-01-10T01:25:26Z
---

# how to set MAX_JOBS for ninja while using uv

---

_Issue opened by @DefTruth on 2025-04-14 07:58_

### Question

how to set MAX_JOBS for ninja while using uv

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @DefTruth on 2025-04-14 07:58_

---

_Comment by @DefTruth on 2025-04-14 07:58_

export MAX_JOBS=32 env not work

---

_Comment by @konstin on 2025-04-14 08:05_

Can you share a minimal reproducible example (https://github.com/astral-sh/uv/issues/9452)?

uv forwards environment variables to build processes, so the usual way for ninja should work through uv, too.

---

_Closed by @DefTruth on 2025-04-14 12:23_

---
