```yaml
number: 603
title: "Consistently use `SystemPath` instead of `Path` in server"
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-06-07T13:39:36Z
updated_at: 2025-07-03T09:48:31Z
url: https://github.com/astral-sh/ty/issues/603
synced_at: 2026-01-10T02:07:36Z
```

# Consistently use `SystemPath` instead of `Path` in server

---

_Issue opened by @MichaReiser on 2025-06-07 13:39_

There are still a few usages of `Path` in the server code. We should replace those usages with `SystemPath` (unless we need the std path)

---

_Label `server` added by @MichaReiser on 2025-06-07 13:39_

---

_Closed by @dhruvmanila on 2025-07-03 09:48_

---
