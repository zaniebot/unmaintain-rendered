```yaml
number: 13982
title: "[red-knot] Minor follow-up on slice expression inference"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/slice-inference-follow-up
created_at: 2024-10-29T19:28:08Z
updated_at: 2024-10-29T19:50:02Z
url: https://github.com/astral-sh/ruff/pull/13982
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Minor follow-up on slice expression inference

---

_Pull request opened by @sharkdp on 2024-10-29 19:28_

## Summary

Minor follow-up to #13917 — thanks @AlexWaygood for the post-merge review.

- [Add SliceLiteralType::as_tuple](https://github.com/astral-sh/ruff/commit/7d623502a6c5a2d2777585985ce0592f7c0b426f)
- [Use .expect() instead of SAFETY comment](https://github.com/astral-sh/ruff/commit/21ebbf2f8b4503aaad422c99eadca5a81d821779)
- [Match on ::try_from result](https://github.com/astral-sh/ruff/commit/d5d7a8171cbe30e41f3d619f4d49f35f8ed993ee)
- [Add TODO comment](https://github.com/astral-sh/ruff/commit/4ab20ba4bbc61250a3606ed206e23844a3b9c47c)

## Test Plan

—

---

_Label `red-knot` added by @sharkdp on 2024-10-29 19:28_

---

_Review requested from @carljm by @sharkdp on 2024-10-29 19:28_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-29 19:28_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-29 19:28_

---

_@carljm approved on 2024-10-29 19:31_

---

_Comment by @carljm on 2024-10-29 19:32_

Looks like clippy wants you to pass `SliceLiteralType` by value.

---

_Merged by @sharkdp on 2024-10-29 19:40_

---

_Closed by @sharkdp on 2024-10-29 19:40_

---

_Branch deleted on 2024-10-29 19:40_

---

_Comment by @AlexWaygood on 2024-10-29 19:48_

Thank you!!

---

_Comment by @github-actions[bot] on 2024-10-29 19:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
