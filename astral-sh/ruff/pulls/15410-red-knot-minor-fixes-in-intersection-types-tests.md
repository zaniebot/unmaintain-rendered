```yaml
number: 15410
title: "[red-knot] Minor fixes in intersection-types tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/minor-fixes-intersection-tests
created_at: 2025-01-10T21:42:46Z
updated_at: 2025-01-10T21:53:05Z
url: https://github.com/astral-sh/ruff/pull/15410
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Minor fixes in intersection-types tests

---

_Pull request opened by @sharkdp on 2025-01-10 21:42_

## Summary

Minor fixes in intersection-types tests

## Test Plan

—


---

_Label `red-knot` added by @sharkdp on 2025-01-10 21:42_

---

_Review requested from @carljm by @sharkdp on 2025-01-10 21:42_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-10 21:42_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-10 21:42_

---

_@sharkdp reviewed on 2025-01-10 21:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:426 on 2025-01-10 21:46_

@carljm You phrased it as "subset". Is it also correct so say "subtype" here?

As in, could we write a property test `∀ X, Y. is_disjoint_from(X, Y) => is_subtype_of(X, ~Y)`?

---

_Comment by @github-actions[bot] on 2025-01-10 21:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:426 on 2025-01-10 21:51_

Yes, subtype is accurate as well! And yes, that property test should be valid.

---

_@carljm approved on 2025-01-10 21:51_

---

_Merged by @sharkdp on 2025-01-10 21:53_

---

_Closed by @sharkdp on 2025-01-10 21:53_

---

_Branch deleted on 2025-01-10 21:53_

---
