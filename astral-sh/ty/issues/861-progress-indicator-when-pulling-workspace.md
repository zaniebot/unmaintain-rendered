```yaml
number: 861
title: Progress indicator when pulling workspace diagnostics
type: issue
state: closed
author: MichaReiser
labels:
  - server
  - wish
assignees: []
created_at: 2025-07-21T12:18:52Z
updated_at: 2025-07-31T19:15:24Z
url: https://github.com/astral-sh/ty/issues/861
synced_at: 2026-01-12T15:54:24Z
```

# Progress indicator when pulling workspace diagnostics

---

_@MichaReiser_

Checking large projects can take a significant amount of time and it isn't clear to users that ty is doing something in the background. For example, checking homeassistant-core takes 52s with a debug build. But there's no indicator for users to observe progress (other that there are simply no diagnostics)

---

_Label `server` added by @MichaReiser on 2025-07-21 12:18_

---

_Label `wish` added by @MichaReiser on 2025-07-21 12:18_

---

_Comment by @dhruvmanila on 2025-07-24 04:57_

Reference: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workDoneProgress

This would use the `workDoneToken` that the server will receive in the payload of the `workspace/diagnostic` request.

---

_Comment by @MichaReiser on 2025-07-31 19:15_

This is now implemented, see https://github.com/astral-sh/ruff/pull/19616

---

_Closed by @MichaReiser on 2025-07-31 19:15_

---
