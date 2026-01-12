```yaml
number: 20429
title: "[`flake8-bugbear`] Add `B912`: `map()` without an explicit `strict=` parameter"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: rule/b912
created_at: 2025-09-16T04:14:39Z
updated_at: 2025-09-19T18:01:44Z
url: https://github.com/astral-sh/ruff/pull/20429
synced_at: 2026-01-12T15:57:01Z
```

# [`flake8-bugbear`] Add `B912`: `map()` without an explicit `strict=` parameter

---

_@danparizher_

## Summary

Implements new rule `B912` that requires the `strict=` argument for `map(...)` calls with two or more iterables on Python 3.14+, following the same pattern as `B905` for `zip()`.

Closes #20057


---

_Marked ready for review by @danparizher on 2025-09-16 04:44_

---

_Comment by @github-actions[bot] on 2025-09-16 04:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2025-09-16 06:43_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/map_without_explicit_strict.rs`:13 on 2025-09-16 12:16_

Can you add somewhere in the documentation that this only applies to versions Python 3.14 and later? And maybe add a link to the section of "What's new" that has this change?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/map_without_explicit_strict.rs`:1 on 2025-09-16 12:25_

This looks very close to `zip_without_explicit_strict.rs`. Could we extract the common logic into a helper function somehow, or are they subtly different?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/map_without_explicit_strict.rs`:94 on 2025-09-16 12:27_

Can we not re-use (or alter and then re-use) the crate-visible function `is_infinite_iterable` here?


---

_@dylwil3 requested changes on 2025-09-16 12:28_

Thanks! This looks good - I wonder if we could reduce some code duplication

---

_Comment by @danparizher on 2025-09-16 18:19_

I extracted common helpers into `strict_utils.rs`:
  - `is_infinite_iterable(...)`
  - `any_infinite_iterables(...)`

Applied across `B905`, `B911`, and `B912` to remove duplicated logic

Let me know if that makes the most sense, happy to adjust naming/placement of the helpers if you prefer a different structure

---

_Review requested from @dylwil3 by @danparizher on 2025-09-16 18:20_

---

_@dylwil3 approved on 2025-09-19 17:33_

Thank you!

---

_Merged by @dylwil3 on 2025-09-19 17:54_

---

_Closed by @dylwil3 on 2025-09-19 17:54_

---

_Label `preview` added by @dylwil3 on 2025-09-19 17:54_

---

_Branch deleted on 2025-09-19 18:01_

---
