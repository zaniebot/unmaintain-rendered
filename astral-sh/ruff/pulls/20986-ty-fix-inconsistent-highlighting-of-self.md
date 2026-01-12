```yaml
number: 20986
title: "[ty] Fix inconsistent highlighting of self"
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
head: micha/fix-inconsistent-self-highlighting
created_at: 2025-10-20T09:20:32Z
updated_at: 2025-10-20T17:32:50Z
url: https://github.com/astral-sh/ruff/pull/20986
synced_at: 2026-01-12T15:57:13Z
```

# [ty] Fix inconsistent highlighting of self

---

_@MichaReiser_

## Summary

Fix the inconsistent highlighting of `self`.

The root cause of the inconsistent highlighting was that the semantic token visitor failed to classify `self.x` (or any other non-name expression) in an annotated assignment. VS Code then applies its default highlighting rules (for any unclassified token). 

The fix is simple, remove the custom handling for `StmtAnnAssign` and override `visit_annotation`, the `SourceOrderVisitor` then does the traversing as expected.

We'd ideally classifiy `self` as `SelfParameter` but that's separate from this bug fix. 

Fixes https://github.com/astral-sh/ty/issues/1126

## Test Plan

Added test, updated existing tests, and...

<img width="502" height="582" alt="Screenshot 2025-10-20 at 11 24 39" src="https://github.com/user-attachments/assets/fd8d80dd-aecf-4e4d-80aa-e5e71665a25d" />


---

_Label `bug` added by @MichaReiser on 2025-10-20 09:20_

---

_Label `server` added by @MichaReiser on 2025-10-20 09:20_

---

_Label `ty` added by @MichaReiser on 2025-10-20 09:20_

---

_Label `bug` added by @MichaReiser on 2025-10-20 09:20_

---

_Label `server` added by @MichaReiser on 2025-10-20 09:20_

---

_Label `ty` added by @MichaReiser on 2025-10-20 09:20_

---

_Marked ready for review by @MichaReiser on 2025-10-20 09:25_

---

_Review requested from @carljm by @MichaReiser on 2025-10-20 09:25_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-20 09:25_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-20 09:25_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-20 09:25_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/semantic_tokens.rs`:2256 on 2025-10-20 11:53_

I still sort-of wish we'd landed https://github.com/astral-sh/ruff/pull/19201 :(

---

_@AlexWaygood approved on 2025-10-20 11:53_

---

_Closed by @MichaReiser on 2025-10-20 12:26_

---

_Reopened by @MichaReiser on 2025-10-20 12:26_

---

_Comment by @github-actions[bot] on 2025-10-20 17:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-20 17:27_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Merged by @MichaReiser on 2025-10-20 17:32_

---

_Closed by @MichaReiser on 2025-10-20 17:32_

---

_Branch deleted on 2025-10-20 17:32_

---
