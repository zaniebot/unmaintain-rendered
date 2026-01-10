```yaml
number: 12761
title: Avoid GitHub fast-path queries when rate limited
type: issue
state: closed
author: zanieb
labels:
  - performance
assignees: []
created_at: 2025-04-08T20:09:19Z
updated_at: 2025-06-24T19:11:42Z
url: https://github.com/astral-sh/uv/issues/12761
synced_at: 2026-01-10T03:32:45Z
```

# Avoid GitHub fast-path queries when rate limited

---

_Issue opened by @zanieb on 2025-04-08 20:09_

Once we receive a 429, it may no longer be a "fast path" to hit GitHub directly (see https://github.com/astral-sh/uv/issues/12746#issuecomment-2787524690). Perhaps we want to add special 429 handling?

---

_Label `performance` added by @zanieb on 2025-04-08 20:09_

---

_Closed by @oconnor663 on 2025-06-24 19:11_

---
