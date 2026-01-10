```yaml
number: 924
title: Server clears and recomputes all workspace diagnostic every 2s
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - server
assignees: []
created_at: 2025-08-01T14:27:13Z
updated_at: 2025-08-04T11:49:39Z
url: https://github.com/astral-sh/ty/issues/924
synced_at: 2026-01-10T02:06:24Z
```

# Server clears and recomputes all workspace diagnostic every 2s

---

_Issue opened by @MichaReiser on 2025-08-01 14:27_

I think this is due to me using a rather special path:

```
file:///Users/micha/astral/ecosystem/langchain-ai:langchain/libs/langchain/langchain/llms/yandex.py
```

What happens is that the first request returns about 2k diagnostics

The second request then fails to match up the previous result ids because `file_to_path` isn't guaranteed to be symmetric with what the server gives us.


The fix here is probably easy. Don't use `file_to_path` when matching against url's provided from the server. Instead, match by file or `AnySystemPath`

---

_Label `bug` added by @MichaReiser on 2025-08-01 14:27_

---

_Label `server` added by @MichaReiser on 2025-08-01 14:27_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-08-01 14:27_

---

_Closed by @MichaReiser on 2025-08-04 11:49_

---
