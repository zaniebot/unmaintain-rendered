```yaml
number: 20354
title: "[ty] guard against recursion in determining completion kind"
type: pull_request
state: merged
author: carljm
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: cjm/lsp-so
created_at: 2025-09-11T18:07:25Z
updated_at: 2025-09-11T18:29:53Z
url: https://github.com/astral-sh/ruff/pull/20354
synced_at: 2026-01-10T17:40:28Z
```

# [ty] guard against recursion in determining completion kind

---

_Pull request opened by @carljm on 2025-09-11 18:07_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1158

## Test Plan

Added test.


---

_Review requested from @AlexWaygood by @carljm on 2025-09-11 18:07_

---

_Label `ty` added by @carljm on 2025-09-11 18:07_

---

_Review requested from @sharkdp by @carljm on 2025-09-11 18:07_

---

_Review requested from @dcreager by @carljm on 2025-09-11 18:07_

---

_Review requested from @MichaReiser by @carljm on 2025-09-11 18:07_

---

_Review requested from @BurntSushi by @carljm on 2025-09-11 18:08_

---

_Label `server` added by @AlexWaygood on 2025-09-11 18:08_

---

_@AlexWaygood approved on 2025-09-11 18:09_

---

_Comment by @github-actions[bot] on 2025-09-11 18:09_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-11 18:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @carljm on 2025-09-11 18:25_

---

_Closed by @carljm on 2025-09-11 18:25_

---

_Branch deleted on 2025-09-11 18:25_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/semantic_model.rs`:367 on 2025-09-11 18:29_

This is neat!

---

_@BurntSushi reviewed on 2025-09-11 18:29_

Cool! TIL about `CycleDetector`. :-)

---
