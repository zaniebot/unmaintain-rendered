```yaml
number: 20312
title: "[ty] introduce `DivergentType`"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: div-type
created_at: 2025-09-09T04:30:15Z
updated_at: 2025-09-12T11:57:59Z
url: https://github.com/astral-sh/ruff/pull/20312
synced_at: 2026-01-10T17:40:28Z
```

# [ty] introduce `DivergentType`

---

_Pull request opened by @mtshiba on 2025-09-09 04:30_

## Summary

From #17371

In #17371, `DivergentType` was introduced to prevent type inference for recursive functions from diverging and causing panics. This turned out to be useful for other divergent type inferences (https://github.com/astral-sh/ruff/pull/17371#discussion_r2329337965), so I extracted the introduction part of `DivergentType` into this PR so that we can use it without waiting for the merge of #17371.

Note that this PR only introduces `DivergentType` and does not actually address divergent type inference yet. Please refer to https://github.com/astral-sh/ruff/pull/17371/files#diff-20b910c6e20faa962bb1642e111db1cbad8e66ace089bdd966ac9d7f9fa99ff2R542-R622 etc. when implementing handling of divergence suppression using this type.

## Test Plan



---

_Comment by @github-actions[bot] on 2025-09-09 04:32_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-09 04:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @mtshiba on 2025-09-09 04:46_

---

_Review requested from @carljm by @mtshiba on 2025-09-09 04:46_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-09-09 04:46_

---

_Review requested from @sharkdp by @mtshiba on 2025-09-09 04:46_

---

_Review requested from @dcreager by @mtshiba on 2025-09-09 04:46_

---

_Label `ty` added by @AlexWaygood on 2025-09-09 07:20_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:7548 on 2025-09-09 18:44_

```suggestion
            | (_, div @ Type::Dynamic(DynamicType::Divergent(_)), _) => Some(div),

```

---

_@carljm reviewed on 2025-09-09 18:45_

---

_@carljm approved on 2025-09-10 15:15_

Looks good, thank you!  I put together a [proof-of-concept](https://github.com/astral-sh/ruff/pull/20333) to show how we can use this type to address an infinite-tuple case in implicit instance attribute (given a [small change in Salsa](https://github.com/salsa-rs/salsa/pull/991)) -- that's enough to convince me that this will be generally useful.

---

_Merged by @carljm on 2025-09-10 15:32_

---

_Closed by @carljm on 2025-09-10 15:32_

---

_Branch deleted on 2025-09-12 11:57_

---
