```yaml
number: 18943
title: "[ty] Reduce the overwhelming complexity of `TypeInferenceBuilder::infer_call_expression`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/refactor-infer-call-expr
created_at: 2025-06-25T19:32:10Z
updated_at: 2025-06-25T20:10:56Z
url: https://github.com/astral-sh/ruff/pull/18943
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Reduce the overwhelming complexity of `TypeInferenceBuilder::infer_call_expression`

---

_Pull request opened by @AlexWaygood on 2025-06-25 19:32_

## Summary

This function is huge, and hugely indented. This PR breaks most of it out into two helper functions: `KnownFunction::check_call()` and `KnownClass::check_call`.

My immediate motivation is that we need to add yet more special cases to this function in order to properly handle `tuple` instantiations and instantiations of tuple subclasses. But I really don't relish the thought of doing that with the function's current structure 😆

## Test Plan

Existing tests all pass. No new ones are added; this is a pure refactor that should have no functional change.


---

_Review requested from @carljm by @AlexWaygood on 2025-06-25 19:32_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-25 19:32_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-25 19:32_

---

_Label `internal` added by @AlexWaygood on 2025-06-25 19:32_

---

_Label `ty` added by @AlexWaygood on 2025-06-25 19:32_

---

_Comment by @github-actions[bot] on 2025-06-25 19:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-06-25 20:09_

---

_Merged by @AlexWaygood on 2025-06-25 20:10_

---

_Closed by @AlexWaygood on 2025-06-25 20:10_

---

_Branch deleted on 2025-06-25 20:10_

---
