```yaml
number: 13643
title: "[red-knot] Improve tests relating to type inference for exception handlers"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: improve-redknot-tests
created_at: 2024-10-05T16:54:28Z
updated_at: 2024-10-05T17:08:46Z
url: https://github.com/astral-sh/ruff/pull/13643
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] Improve tests relating to type inference for exception handlers

---

_Pull request opened by @AlexWaygood on 2024-10-05 16:54_

## Summary

As currently written, these tests break if you try to improve exception-handler control flow à la astral-sh/ruff#13633, and they'll also break when we fix astral-sh/ty#238. This PR uses `reveal_type` to make them robust to future improvements in our understanding of exception-handler control-flow.

## Test Plan

`cargo test -p red_knot_python_semantic --lib`


---

_Label `red-knot` added by @AlexWaygood on 2024-10-05 16:54_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-05 16:54_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-05 16:54_

---

_Merged by @AlexWaygood on 2024-10-05 16:59_

---

_Closed by @AlexWaygood on 2024-10-05 16:59_

---

_Branch deleted on 2024-10-05 16:59_

---

_Comment by @github-actions[bot] on 2024-10-05 17:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
