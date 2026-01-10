```yaml
number: 14838
title: "[red-knot] Improve type inference for except handlers"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/except-handlers
created_at: 2024-12-08T17:08:55Z
updated_at: 2024-12-09T22:50:00Z
url: https://github.com/astral-sh/ruff/pull/14838
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Improve type inference for except handlers

---

_Pull request opened by @AlexWaygood on 2024-12-08 17:08_

## Summary

I noticed in https://github.com/astral-sh/ruff/pull/14802#discussion_r1873187227 that there were some TODOs around type inference for exception handlers that we could now do, following @sharkdp's implementation of `Type::SubclassOf`. So this PR does them!

Features added in this PR:
- Diagnostics for invalid types caught in (sync or async) exception handlers
- Support for dynamic exception handlers (e.g. catching a value where the binding comes from a parameter annotated as `type[ValueError]` or `tuple[type[ValueError], type[OSError]]` is now supported)

## Test Plan

New mdtests added, and TODOs in existing mdtests removed.


---

_Label `red-knot` added by @AlexWaygood on 2024-12-08 17:08_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-08 17:08_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-08 17:08_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-08 17:08_

---

_Comment by @github-actions[bot] on 2024-12-08 17:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-12-09 21:30_

Excellent!

---

_Merged by @AlexWaygood on 2024-12-09 22:49_

---

_Closed by @AlexWaygood on 2024-12-09 22:49_

---

_Branch deleted on 2024-12-09 22:50_

---
