```yaml
number: 16253
title: "Rename `ExprStringLiteral::as_unconcatenated_string()` to `ExprStringLiteral::as_single_part_string()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/as-single-part-string
created_at: 2025-02-19T14:33:24Z
updated_at: 2025-02-19T16:07:00Z
url: https://github.com/astral-sh/ruff/pull/16253
synced_at: 2026-01-12T15:55:54Z
```

# Rename `ExprStringLiteral::as_unconcatenated_string()` to `ExprStringLiteral::as_single_part_string()`

---

_@AlexWaygood_

## Summary

This addresses feedback in https://github.com/astral-sh/ruff/pull/16192#discussion_r1957752577 that the name of the new `ExprStringLiteral::as_unconcatenated_literal()` method was somewhat confusing, given that the variants of the inner enum are `StringLiteralValueInner::Single()` and `StringLiteralValueInner::Concatenated()`. The method is renamed to `ExprStringLiteral::as_single_part_string()`, which I _hope_ is something we can compromise on ;)

I also added similar methods to `ExprFString` and `ExprBytesLiteral`, and adapted a few more callsites to use these methods.

## Test Plan

`cargo test`


---

_Label `internal` added by @AlexWaygood on 2025-02-19 14:33_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-19 14:33_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-19 14:33_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-19 14:33_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-02-19 14:33_

---

_Comment by @github-actions[bot] on 2025-02-19 14:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser approved on 2025-02-19 15:51_

---

_Merged by @AlexWaygood on 2025-02-19 16:06_

---

_Closed by @AlexWaygood on 2025-02-19 16:06_

---

_Branch deleted on 2025-02-19 16:07_

---
