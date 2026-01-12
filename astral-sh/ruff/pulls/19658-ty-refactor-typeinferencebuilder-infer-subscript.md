```yaml
number: 19658
title: "[ty] Refactor `TypeInferenceBuilder::infer_subscript_expression_types`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/binary-method-fallbacks
created_at: 2025-07-31T11:17:56Z
updated_at: 2025-07-31T13:39:16Z
url: https://github.com/astral-sh/ruff/pull/19658
synced_at: 2026-01-12T15:56:44Z
```

# [ty] Refactor `TypeInferenceBuilder::infer_subscript_expression_types`

---

_@AlexWaygood_

## Summary

This PR refactors the generic fallback `match` arm in `TypeInferenceBuilder::infer_subscript_expression_types` into a closure that can be called from multiple places. This refactor has several advantages:
- It's marginally less code, and it leads to cleaner code in many places: we need to have fewer `.expect("Checked in branch arm")` calls peppered across the function
- It gives a (small, but very consistent) [1% speedup on many of the benchmarks](https://codspeed.io/astral-sh/ruff/branches/alex%2Fbinary-method-fallbacks?runnerMode=Instrumentation). I think this is mostly because we no longer need to eagerly call `.slice_literal(db)` at the top of the function; instead, we only do it in the branch arms for `Type` variants that we know could plausibly represent a `slice` instance type
- It makes further changes that I'd like to do (relating to https://github.com/astral-sh/ty/issues/545) easier

## Test Plan

All existing tests pass


---

_Label `ty` added by @AlexWaygood on 2025-07-31 11:17_

---

_Comment by @github-actions[bot] on 2025-07-31 11:19_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-31 11:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-07-31 12:01_

---

_Review requested from @carljm by @AlexWaygood on 2025-07-31 12:01_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-31 12:01_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-31 12:01_

---

_Label `internal` added by @AlexWaygood on 2025-07-31 12:01_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:8491 on 2025-07-31 12:55_

I guess an alternative to a function would be that the match arms return an `Option` or call `return` if they can infer a type. 

We could then put the fallback logic at the end of this method 

---

_@MichaReiser approved on 2025-07-31 12:55_

---

_@AlexWaygood reviewed on 2025-07-31 13:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:8491 on 2025-07-31 13:04_

Oh, that's a nice idea!

---

_Merged by @AlexWaygood on 2025-07-31 13:38_

---

_Closed by @AlexWaygood on 2025-07-31 13:38_

---

_Branch deleted on 2025-07-31 13:38_

---
