```yaml
number: 19944
title: "[ty] Have `SemanticIndex::place_table()` and `SemanticIndex::use_def_map` return references"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/refactor-use-def
created_at: 2025-08-16T20:56:54Z
updated_at: 2025-08-18T10:38:02Z
url: https://github.com/astral-sh/ruff/pull/19944
synced_at: 2026-01-12T15:56:51Z
```

# [ty] Have `SemanticIndex::place_table()` and `SemanticIndex::use_def_map` return references

---

_@AlexWaygood_

## Summary

The fact that these return owned `Arc`s has already caused a number of borrow-checker issues that have been annoying to workaround. And I just ran into this yet again while working on another change.

Everything _seems_ to compile if we return `&Arc` from these methods rather than `Arc`, and (since the implicit lifetime of the borrow is `'db`) this solves a lot of borrow-checker issues!

## Test Plan

Existing tests


---

_Label `ty` added by @AlexWaygood on 2025-08-16 20:56_

---

_Label `internal` added by @AlexWaygood on 2025-08-16 20:57_

---

_Comment by @github-actions[bot] on 2025-08-16 20:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-16 21:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-08-16 21:09_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-16 21:09_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-16 21:09_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-16 21:09_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-08-16 21:50_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index.rs`:253 on 2025-08-18 06:35_

Returning an `&Arc` seems a bit weird and I suspect we only do it for the `place_table` query. I suggest returning a `&PlaceTable` here and a) have a `place_table_arc` method instead that returns a cloned `Arc` OR just copy paste this one liner into the `place_table` query. The same for `use_def_map`.

---

_@MichaReiser approved on 2025-08-18 06:36_

thank you

---

_Merged by @AlexWaygood on 2025-08-18 10:30_

---

_Closed by @AlexWaygood on 2025-08-18 10:30_

---

_Branch deleted on 2025-08-18 10:30_

---

_Comment by @github-actions[bot] on 2025-08-18 10:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
