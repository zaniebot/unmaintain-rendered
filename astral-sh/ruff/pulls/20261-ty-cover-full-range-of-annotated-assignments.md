```yaml
number: 20261
title: "[ty] Cover full range of annotated assignments"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: david/annoated-assignment-full-range
created_at: 2025-09-05T07:53:07Z
updated_at: 2025-09-05T08:12:42Z
url: https://github.com/astral-sh/ruff/pull/20261
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Cover full range of annotated assignments

---

_Pull request opened by @sharkdp on 2025-09-05 07:53_

## Summary

An annotated assignment `name: annotation` without a right-hand side was previously not covered by the range returned from `DefinitionKind::full_range`, because we did expand the range to include the right-hand side (if there was one), but failed to include the annotation.

## Test Plan

Updated snapshot tests


---

_Review requested from @carljm by @sharkdp on 2025-09-05 07:53_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-05 07:53_

---

_Review requested from @dcreager by @sharkdp on 2025-09-05 07:53_

---

_Label `internal` added by @sharkdp on 2025-09-05 07:53_

---

_Label `ty` added by @sharkdp on 2025-09-05 07:53_

---

_Label `diagnostics` added by @sharkdp on 2025-09-05 07:53_

---

_Comment by @github-actions[bot] on 2025-09-05 07:55_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-05 07:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp reviewed on 2025-09-05 08:00_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/definition.rs`:773 on 2025-09-05 08:00_

Due to how the syntax is structured, we could avoid this step if there is a right hand side (because covering the target and the right-hand side will necessarily also include the annotation). But this method is not performance critical, and I find it easier to reason about in this form: just always include the annotation.

---

_@AlexWaygood approved on 2025-09-05 08:02_

---

_Merged by @sharkdp on 2025-09-05 08:12_

---

_Closed by @sharkdp on 2025-09-05 08:12_

---

_Branch deleted on 2025-09-05 08:12_

---
