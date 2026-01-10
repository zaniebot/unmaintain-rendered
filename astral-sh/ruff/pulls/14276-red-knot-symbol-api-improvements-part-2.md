```yaml
number: 14276
title: "[red-knot] Symbol API improvements, part 2"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/symbol-api-part2
created_at: 2024-11-11T12:59:43Z
updated_at: 2024-11-11T17:31:26Z
url: https://github.com/astral-sh/ruff/pull/14276
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Symbol API improvements, part 2

---

_Pull request opened by @sharkdp on 2024-11-11 12:59_

## Summary

Apart from one small functional change, this is mostly a refactoring of the `Symbol` API:

- Rename `as_type` to the more explicit `ignore_possibly_unbound`, no functional change
- Remove `unwrap_or_unknown` in favor of the more explicit `.ignore_possibly_unbound().unwrap_or(Type::Unknown)`, no functional change
- Consistently call it "possibly unbound" (not "may be unbound")
- Rename `replace_unbound_with` to `or_fall_back_to` and properly handle boundness of the fall back. This is the only functional change (did not have any impact on existing tests).

relates to: #14022

## Test Plan

New unit tests for `Symbol::or_fall_back_to`


---

_Label `red-knot` added by @sharkdp on 2024-11-11 12:59_

---

_Review requested from @carljm by @sharkdp on 2024-11-11 12:59_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-11 12:59_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-11 12:59_

---

_Comment by @github-actions[bot] on 2024-11-11 13:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:78 on 2024-11-11 14:00_

```suggestion
                Symbol::Type(_, Boundness::Bound) => self,
```

---

_@AlexWaygood approved on 2024-11-11 14:05_

Thank you! This makes things much more explicit.

---

_Merged by @sharkdp on 2024-11-11 14:24_

---

_Closed by @sharkdp on 2024-11-11 14:24_

---

_Branch deleted on 2024-11-11 14:24_

---

_Comment by @carljm on 2024-11-11 17:31_

Much clearer, thank you!

---
