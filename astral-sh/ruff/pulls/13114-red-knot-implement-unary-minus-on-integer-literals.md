```yaml
number: 13114
title: "red-knot: implement unary minus on integer literals"
type: pull_request
state: merged
author: chriskrycho
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-infer-unary-negate-int-literal
created_at: 2024-08-26T18:50:19Z
updated_at: 2024-08-27T23:44:54Z
url: https://github.com/astral-sh/ruff/pull/13114
synced_at: 2026-01-10T21:38:32Z
```

# red-knot: implement unary minus on integer literals

---

_Pull request opened by @chriskrycho on 2024-08-26 18:50_

# Summary

Add support for the first unary operator: negating integer literals. The resulting type is another integer literal, with the value being the negated value of the literal. All other types continue to return `Type::Unknown` for the present, but this is designed to make it easy to extend easily with other combinations of operator and operand.

Contributes to astral-sh/ty#244.

## Test Plan

Add tests with basic negation, including of very large integers and double negation.


---

_Review requested from @carljm by @chriskrycho on 2024-08-26 18:50_

---

_Review requested from @MichaReiser by @chriskrycho on 2024-08-26 18:50_

---

_Review requested from @AlexWaygood by @chriskrycho on 2024-08-26 18:50_

---

_@carljm approved on 2024-08-26 18:51_

Looks great!

---

_@AlexWaygood approved on 2024-08-26 18:53_

---

_Comment by @github-actions[bot] on 2024-08-26 19:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-08-26 19:08_

---

_Closed by @carljm on 2024-08-26 19:08_

---

_Branch deleted on 2024-08-26 19:08_

---

_Label `red-knot` added by @carljm on 2024-08-27 23:44_

---
