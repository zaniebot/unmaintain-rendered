```yaml
number: 19691
title: "[ty] Don't send unchanged workspace document reports"
type: pull_request
state: closed
author: MichaReiser
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: micha/dont-send-unchanged
created_at: 2025-08-01T15:32:19Z
updated_at: 2025-11-16T11:47:16Z
url: https://github.com/astral-sh/ruff/pull/19691
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Don't send unchanged workspace document reports

---

_Pull request opened by @MichaReiser on 2025-08-01 15:32_

## Summary

The spec isn't explicit about whether they are required but Roslyn's LSP isn't sending them 
and the diagnostics are reported just fine in VS code and Zed if I remove the events (which sort of makes sense). 

## Test Plan

<!-- How was it tested? -->


---

_Label `server` added by @MichaReiser on 2025-08-01 15:32_

---

_Label `ty` added by @MichaReiser on 2025-08-01 15:32_

---

_Label `server` added by @MichaReiser on 2025-08-01 15:32_

---

_Label `ty` added by @MichaReiser on 2025-08-01 15:32_

---

_Comment by @github-actions[bot] on 2025-08-01 15:34_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-01 15:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MichaReiser on 2025-08-07 10:37_

I'll close this for now because I don't have a good way for testing bulk workspace diagnostics and I'm not sure if that won't break any client (as the spec is very unclear if omitting them is fine). We can revisit this later

---

_Closed by @MichaReiser on 2025-08-07 10:37_

---

_Branch deleted on 2025-11-16 11:47_

---
