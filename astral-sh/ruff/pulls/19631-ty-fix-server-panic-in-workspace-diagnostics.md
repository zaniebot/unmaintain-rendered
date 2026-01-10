```yaml
number: 19631
title: "[ty] Fix server panic in workspace diagnostics request handler when typing"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/workspace-diagnostic-panic
created_at: 2025-07-30T08:43:09Z
updated_at: 2025-07-30T15:40:43Z
url: https://github.com/astral-sh/ruff/pull/19631
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Fix server panic in workspace diagnostics request handler when typing

---

_Pull request opened by @MichaReiser on 2025-07-30 08:43_

## Summary

Fixes https://github.com/astral-sh/ty/issues/911

Similar to https://github.com/astral-sh/ruff/pull/18252

The `Drop` relative drop order between the `Database` and `Arc`'s that we call `into_inner` on (require exactly one reference)
matter because Salsa's cancellation only waits for the Db to drop. That means, the db must come last! 

I don't know how to avoid this more systematically but I'm also not convinced that we have to. 
There are only very few `Arc`'s where this matters (just `Index` really) and there are very few places where it is used.

## Test Plan

I typed like a mad man while the workspace diagnostic request was in flight and I haven't been able to reproduce the crash anymore


---

_Label `server` added by @MichaReiser on 2025-07-30 08:43_

---

_Label `ty` added by @MichaReiser on 2025-07-30 08:43_

---

_Review requested from @carljm by @MichaReiser on 2025-07-30 08:43_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-30 08:43_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-30 08:43_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-30 08:43_

---

_Label `server` added by @MichaReiser on 2025-07-30 08:43_

---

_Label `ty` added by @MichaReiser on 2025-07-30 08:43_

---

_Review request for @dcreager removed by @MichaReiser on 2025-07-30 08:43_

---

_Review request for @carljm removed by @MichaReiser on 2025-07-30 08:43_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-07-30 08:43_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-07-30 08:43_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-07-30 08:43_

---

_@MichaReiser reviewed on 2025-07-30 08:44_

---

_Review comment by @MichaReiser on `crates/ty_server/src/session.rs`:106 on 2025-07-30 08:44_

This change isn't necessary but it hopefully avoids similar issues in the future

---

_Comment by @github-actions[bot] on 2025-07-30 08:45_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-30 08:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@dhruvmanila approved on 2025-07-30 14:48_

Thank you

---

_Merged by @MichaReiser on 2025-07-30 15:40_

---

_Closed by @MichaReiser on 2025-07-30 15:40_

---

_Branch deleted on 2025-07-30 15:40_

---
