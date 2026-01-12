```yaml
number: 18510
title: "[refurb] Stabilize `fromisoformat-replace-z` (FURB162)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: brent/stabilize-furb162
created_at: 2025-06-06T18:28:52Z
updated_at: 2025-06-10T20:11:30Z
url: https://github.com/astral-sh/ruff/pull/18510
synced_at: 2026-01-12T15:56:20Z
```

# [refurb] Stabilize `fromisoformat-replace-z` (FURB162)

---

_@ntBre_

This PR stabilizes the FURB162 rule by moving it from preview to stable status for the 0.12.0 release.

## Summary
- **Rule**: FURB162 (`fromisoformat-replace-z`)
- **Purpose**: Detects unnecessary timezone replacement operations when calling `datetime.fromisoformat()`
- **Change**: Move from `RuleGroup::Preview` to `RuleGroup::Stable` in `codes.rs`

## Verification Links
- **Tests**: [refurb/mod.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/refurb/mod.rs#L54) - Confirms FURB162 has only standard tests, no preview-specific test cases
- **Documentation**: https://docs.astral.sh/ruff/rules/fromisoformat-replace-z/ - Current documentation shows preview status that will be automatically updated

---

_Comment by @github-actions[bot] on 2025-06-06 18:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-06-06 18:39_

---

_@dylwil3 approved on 2025-06-06 21:28_

---

_Merged by @ntBre on 2025-06-06 21:44_

---

_Closed by @ntBre on 2025-06-06 21:44_

---

_Branch deleted on 2025-06-06 21:44_

---

_Added to milestone `v0.12` by @ntBre on 2025-06-10 20:11_

---
