```yaml
number: 18512
title: "[ruff] Stabilize `class-with-mixed-type-vars` (RUF053)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: brent/stabilize-ruf053
created_at: 2025-06-06T19:16:45Z
updated_at: 2025-06-10T20:11:25Z
url: https://github.com/astral-sh/ruff/pull/18512
synced_at: 2026-01-10T18:45:04Z
```

# [ruff] Stabilize `class-with-mixed-type-vars` (RUF053)

---

_Pull request opened by @ntBre on 2025-06-06 19:16_

This PR stabilizes the RUF053 rule by moving it from preview to stable status for the 0.12.0 release.

## Summary
- **Rule**: RUF053 (`class-with-mixed-type-vars`)
- **Purpose**: Detects classes that have both PEP 695 type parameter lists while also inheriting from `typing.Generic`
- **Change**: Move from `RuleGroup::Preview` to `RuleGroup::Stable` in `codes.rs` and migrate preview tests to stable tests

## Verification Links
- **Tests**: [ruff/mod.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/mod.rs#L98) - Shows RUF053 moved from preview_rules to main rules test function
- **Documentation**: https://docs.astral.sh/ruff/rules/class-with-mixed-type-vars/ - Current documentation shows preview status that will be automatically updated

---

_Comment by @github-actions[bot] on 2025-06-06 20:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-06-06 20:47_

---

_@dylwil3 approved on 2025-06-06 21:11_

LGTM!

---

_Merged by @ntBre on 2025-06-06 21:21_

---

_Closed by @ntBre on 2025-06-06 21:21_

---

_Branch deleted on 2025-06-06 21:21_

---

_Added to milestone `v0.12` by @ntBre on 2025-06-10 20:11_

---
