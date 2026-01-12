```yaml
number: 20039
title: "[ty] Cancel background tasks when shutdown is requested"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/shutdown-cancel-background-tasks
created_at: 2025-08-22T07:39:34Z
updated_at: 2025-08-22T08:20:14Z
url: https://github.com/astral-sh/ruff/pull/20039
synced_at: 2026-01-12T15:56:53Z
```

# [ty] Cancel background tasks when shutdown is requested

---

_@MichaReiser_

## Summary

Cancel any background tasks when the client requests the server to shut down. 

This ensures that long running tasks like workspace diagnostics or workspace symbols don't run to completion before shutting down the server.

## Test Plan

Restarted the server while workspace diagnostics was running. The server exited immediately.


---

_Label `bug` added by @MichaReiser on 2025-08-22 07:39_

---

_Label `server` added by @MichaReiser on 2025-08-22 07:39_

---

_Label `ty` added by @MichaReiser on 2025-08-22 07:39_

---

_Review requested from @carljm by @MichaReiser on 2025-08-22 07:39_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-22 07:39_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-22 07:39_

---

_Comment by @github-actions[bot] on 2025-08-22 07:41_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-22 07:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-08-22 08:15_

---

_Merged by @MichaReiser on 2025-08-22 08:20_

---

_Closed by @MichaReiser on 2025-08-22 08:20_

---

_Branch deleted on 2025-08-22 08:20_

---
