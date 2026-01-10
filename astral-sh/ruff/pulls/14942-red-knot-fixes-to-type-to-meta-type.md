```yaml
number: 14942
title: "[red-knot] Fixes to `Type::to_meta_type`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/any-meta-type
created_at: 2024-12-12T18:54:54Z
updated_at: 2024-12-12T19:55:13Z
url: https://github.com/astral-sh/ruff/pull/14942
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Fixes to `Type::to_meta_type`

---

_Pull request opened by @AlexWaygood on 2024-12-12 18:54_

## Summary

If a symbol `x` has type `Any`, `x.__class__` should still be a dynamic, unknown type. However, that doesn't mean that it has to be `Any` _exactly_. We know that for any object `x`, `x.__class__` will be an instance of `type`; if `x` has type `Any`, this still holds true, we just don't know which _exact_ instance of `type` we have for `x.__class__`. This can be expressed using the type `type[Any]`.

To implement these semantics, this PR modifies `Type::to_meta_type` so that the meta-type of `Any` is `type[Any]`, the meta-type of `Unknown` is `type[Unknown]` and the meta-type of `Todo` is `type[Todo]`.

## Test Plan

New mdtests added


---

_Label `red-knot` added by @AlexWaygood on 2024-12-12 18:54_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-12 18:54_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-12 18:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-12 18:54_

---

_Comment by @github-actions[bot] on 2024-12-12 19:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-12-12 19:53_

Excellent!

---

_Merged by @AlexWaygood on 2024-12-12 19:55_

---

_Closed by @AlexWaygood on 2024-12-12 19:55_

---

_Branch deleted on 2024-12-12 19:55_

---
