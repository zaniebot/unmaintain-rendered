```yaml
number: 21057
title: "[ty] Add comment explaining why `HasTrackedScope` is implemented for `Identifier` and why it works"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/identifier-tracked-scoipe
created_at: 2025-10-24T07:16:51Z
updated_at: 2025-10-24T07:48:59Z
url: https://github.com/astral-sh/ruff/pull/21057
synced_at: 2026-01-12T15:57:15Z
```

# [ty] Add comment explaining why `HasTrackedScope` is implemented for `Identifier` and why it works

---

_@MichaReiser_

## Summary

I always assumed that the lookup by identifier is just a no-op and doesn't work. But it does! 

This was the last clean-up that I wanted to do for https://github.com/astral-sh/ty/issues/572, which I think we can close now

## Test Plan

Alex


---

_Review requested from @carljm by @MichaReiser on 2025-10-24 07:16_

---

_Label `internal` added by @MichaReiser on 2025-10-24 07:16_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-24 07:16_

---

_Label `ty` added by @MichaReiser on 2025-10-24 07:16_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-24 07:16_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-24 07:16_

---

_Comment by @github-actions[bot] on 2025-10-24 07:21_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-24 07:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-10-24 07:47_

---

_Merged by @MichaReiser on 2025-10-24 07:48_

---

_Closed by @MichaReiser on 2025-10-24 07:48_

---

_Branch deleted on 2025-10-24 07:48_

---
