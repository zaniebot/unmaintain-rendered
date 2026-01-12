```yaml
number: 21190
title: "[`refurb`] Fix false negative for underscores before sign in `Decimal` constructor (`FURB157`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-21186
created_at: 2025-11-01T19:26:35Z
updated_at: 2025-11-04T16:13:38Z
url: https://github.com/astral-sh/ruff/pull/21190
synced_at: 2026-01-12T15:57:18Z
```

# [`refurb`] Fix false negative for underscores before sign in `Decimal` constructor (`FURB157`)

---

_@danparizher_

## Summary

Fixes FURB157 false negative where `Decimal("_-1")` was not flagged as verbose when underscores precede the sign character. This fixes #21186.

## Problem Analysis

The `verbose-decimal-constructor` (FURB157) rule failed to detect verbose `Decimal` constructors when the sign character (`+` or `-`) was preceded by underscores. For example, `Decimal("_-1")` was not flagged, even though it can be simplified to `Decimal(-1)`.

The bug occurred because the rule checked for the sign character at the start of the string before stripping leading underscores. According to Python's `Decimal` parser behavior (as documented in CPython's `_pydecimal.py`), underscores are removed before parsing the sign. The rule's logic didn't match this behavior, causing a false negative for cases like `"_-1"` where the underscore came before the sign.

This was a regression introduced in version 0.14.3, as these cases were correctly flagged in version 0.14.2.

## Approach

The fix updates the sign extraction logic to:
1. Strip leading underscores first (matching Python's Decimal parser behavior)
2. Extract the sign from the underscore-stripped string
3. Preserve the string after the sign for normalization purposes

This ensures that cases like `Decimal("_-1")`, `Decimal("_+1")`, and `Decimal("_-1_000")` are correctly detected and flagged. The normalization logic was also updated to use the string after the sign (without underscores) to avoid double signs in the replacement output.


---

_Comment by @github-actions[bot] on 2025-11-01 19:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-11-03 21:43_

---

_Label `rule` added by @ntBre on 2025-11-03 21:43_

---

_@ntBre approved on 2025-11-04 16:01_

Thanks, this makes sense to me!

---

_Merged by @ntBre on 2025-11-04 16:02_

---

_Closed by @ntBre on 2025-11-04 16:02_

---

_Branch deleted on 2025-11-04 16:13_

---
