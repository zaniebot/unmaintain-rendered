```yaml
number: 19431
title: "[ty] Reduce size of empty `TypeCheckDiagnostics` to one word"
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
base: main
head: micha/shrink-type-check-diagnostics
created_at: 2025-07-19T15:29:11Z
updated_at: 2025-07-20T08:40:15Z
url: https://github.com/astral-sh/ruff/pull/19431
synced_at: 2026-01-12T15:56:39Z
```

# [ty] Reduce size of empty `TypeCheckDiagnostics` to one word

---

_@MichaReiser_

## Summary

`TypeCheckDiagnostics` are stored in every `TypeInference` and ty creates a lot of them (see below for some numbers from a large project). 
But most of those inference contexts don't have any diagnostics or suppressions.

This PR changes `TypeCheckDiagnostics` to use a `Option<Box<Inner>>` instead. This reduces the size of `TypeInference` from 224 bytes to 176 bytes (48 bytes). 
The downside of this is that it requires an extra allocation for files that use suppressions or diagnostics. But these should be rare.

```
`infer_definition_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=850.32MB fields=2307.24MB count=5089303
`infer_expression_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=1701.13MB fields=2014.07MB count=4971063
`infer_scope_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=400.87MB fields=1738.46MB count=868855
```

I considered using a `ThinVec` for `diagnostics` over boxing the entire `TypeCheckDiagnostics` but that only reduces the size by 16 bytes because
it doesn't help with the much larger `HashSet`.

If the above project would have zero suppressions and diagnostics, this PR saves ~500MB even though ty emits a ton of diagnostics!

```
`infer_definition_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=850.28MB fields=2065.54MB count=5089306
`infer_expression_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=1701.02MB fields=1779.32MB count=4971063
`infer_scope_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=400.87MB fields=1699.84MB count=868855
```

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-07-19 15:29_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-19 15:29_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-19 15:29_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-19 15:29_

---

_Label `internal` added by @MichaReiser on 2025-07-19 15:29_

---

_Label `ty` added by @MichaReiser on 2025-07-19 15:29_

---

_Comment by @github-actions[bot] on 2025-07-19 15:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Renamed from "[ty] Reduce size of `TypeCheckDiagnostics` to one word" to "[ty] Reduce size of empty `TypeCheckDiagnostics` to one word" by @MichaReiser on 2025-07-19 15:47_

---

_Comment by @MichaReiser on 2025-07-20 08:40_

I'll  roll this into another PR.  

---

_Closed by @MichaReiser on 2025-07-20 08:40_

---
