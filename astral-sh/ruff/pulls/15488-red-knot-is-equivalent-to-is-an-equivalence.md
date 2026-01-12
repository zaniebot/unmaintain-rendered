```yaml
number: 15488
title: "[red-knot] 'is_equivalent_to' is an equivalence relation"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/equivalent_to-property-tests
created_at: 2025-01-15T08:13:49Z
updated_at: 2025-01-15T14:54:50Z
url: https://github.com/astral-sh/ruff/pull/15488
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] 'is_equivalent_to' is an equivalence relation

---

_@sharkdp_

## Summary

Adds two additional tests for `is_equivalent_to` so that we cover all properties of an [equivalence relation].

## Test Plan

```
while cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable; do :; done
```

[equivalence relation]: https://en.wikipedia.org/wiki/Equivalence_relation

---

_Label `red-knot` added by @sharkdp on 2025-01-15 08:13_

---

_Review requested from @carljm by @sharkdp on 2025-01-15 08:13_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-15 08:13_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-15 08:13_

---

_@sharkdp reviewed on 2025-01-15 08:14_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:381 on 2025-01-15 08:14_

Unrelated minor fix.

---

_Comment by @github-actions[bot] on 2025-01-15 08:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review request for @MichaReiser removed by @MichaReiser on 2025-01-15 08:20_

---

_Merged by @sharkdp on 2025-01-15 08:25_

---

_Closed by @sharkdp on 2025-01-15 08:25_

---

_Branch deleted on 2025-01-15 08:25_

---

_@dcreager reviewed on 2025-01-15 14:54_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/property_tests.rs`:253 on 2025-01-15 14:54_

I love the syntax of these property tests!

---
