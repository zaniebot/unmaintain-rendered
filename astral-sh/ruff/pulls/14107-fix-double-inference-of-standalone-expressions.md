```yaml
number: 14107
title: fix double inference of standalone expressions
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/standalone-expressions
created_at: 2024-11-05T12:33:40Z
updated_at: 2024-11-05T14:50:33Z
url: https://github.com/astral-sh/ruff/pull/14107
synced_at: 2026-01-12T15:55:46Z
```

# fix double inference of standalone expressions

---

_@MichaReiser_

## Summary

I noticed that migrating to salsa accumulators resulted in duplicated diagnostics because a lot of type inference code incorrectly called `self.infer_expression` instead of `infer_expression_types` for standalone expressions. 

This PR adds a debug assertion to `infer_expression` that throws when called for a standalone expression and it fixes all call sites to correctly use `infer_standalone_expression` instead.

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2024-11-05 12:41_

---

_Renamed from "Avoid double inferring standalone expressions" to "fix double inference of standalone expressions" by @MichaReiser on 2024-11-05 12:45_

---

_Marked ready for review by @MichaReiser on 2024-11-05 12:52_

---

_Review requested from @carljm by @MichaReiser on 2024-11-05 12:52_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-05 12:52_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-05 12:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1972 on 2024-11-05 13:01_

The `Debug` representations of some of our `Expr` nodes can be huge; this might result in a colossal wall of text being printed to the terminal if the assertion fails. I wonder if just printing the type of the variant might be better here?

---

_Comment by @github-actions[bot] on 2024-11-05 13:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-11-05 13:07_

---

_Merged by @MichaReiser on 2024-11-05 14:50_

---

_Closed by @MichaReiser on 2024-11-05 14:50_

---

_Branch deleted on 2024-11-05 14:50_

---
