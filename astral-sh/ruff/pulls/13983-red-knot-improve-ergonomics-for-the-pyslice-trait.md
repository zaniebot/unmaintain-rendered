```yaml
number: 13983
title: "[red-knot] Improve ergonomics for the `PySlice` trait"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/slice-trait
created_at: 2024-10-29T20:32:48Z
updated_at: 2024-10-29T21:09:55Z
url: https://github.com/astral-sh/ruff/pull/13983
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] Improve ergonomics for the `PySlice` trait

---

_@AlexWaygood_

## Summary

Just a minor change to improve the ergonomics of the `PySlice` trait. By implementing `PySlice` for `[T]` rather than `&[T]`, we can call it on `Vec<T>` and `Box<[T]>` without having to have intermediate calls to `.as_slice()` or `.as_ref()`. It can still be called on `&[T]` and `&Box<[T]>`, because the `py_slice` trait method takes `&self`.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Review requested from @sharkdp by @AlexWaygood on 2024-10-29 20:32_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-29 20:32_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-29 20:32_

---

_Label `red-knot` added by @AlexWaygood on 2024-10-29 20:32_

---

_@carljm approved on 2024-10-29 20:39_

Nice!

---

_Merged by @AlexWaygood on 2024-10-29 20:40_

---

_Closed by @AlexWaygood on 2024-10-29 20:40_

---

_Branch deleted on 2024-10-29 20:41_

---

_Comment by @github-actions[bot] on 2024-10-29 20:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sharkdp on 2024-10-29 21:09_

Thank you.

---
