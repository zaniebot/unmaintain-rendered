```yaml
number: 20532
title: "Use `Annotation::tags` instead of hard coding the rules in ruff server"
type: issue
state: closed
author: MichaReiser
labels:
  - internal
  - help wanted
  - server
assignees: []
created_at: 2025-09-23T13:18:27Z
updated_at: 2025-09-26T07:06:27Z
url: https://github.com/astral-sh/ruff/issues/20532
synced_at: 2026-01-10T11:09:59Z
```

# Use `Annotation::tags` instead of hard coding the rules in ruff server

---

_Issue opened by @MichaReiser on 2025-09-23 13:18_

Instead of hard coding the rules in the ruff server:

https://github.com/astral-sh/ruff/blob/a3ec8ca9dfb484775f38712681d1174260d2bf4b/crates/ruff_server/src/lint.rs#L341-L349

Set the `unnecessary` tag on the diagnostic's primary annotation when creating the `"F401" | "F841" | "RUF059"` diagnostics

---

_Label `internal` added by @MichaReiser on 2025-09-23 13:18_

---

_Label `help wanted` added by @MichaReiser on 2025-09-23 13:18_

---

_Label `server` added by @MichaReiser on 2025-09-23 13:18_

---

_Closed by @MichaReiser on 2025-09-26 07:06_

---
