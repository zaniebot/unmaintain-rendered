```yaml
number: 22613
title: Combine suppression code diagnostics
type: pull_request
state: open
author: amyreese
labels:
  - rule
  - suppression
assignees: []
base: main
head: amy/suppression-grouping
created_at: 2026-01-16T00:34:42Z
updated_at: 2026-01-16T08:36:49Z
url: https://github.com/astral-sh/ruff/pull/22613
synced_at: 2026-01-16T08:55:18Z
```

# Combine suppression code diagnostics

---

_@amyreese_

# Summary

- Report a single combined UnusedNOQA diagnostic when any range suppression
  has one or more unused, disabled, or duplicate lint codes.
- Report a single combined InvalidRuleCode diagnostic for any range suppression
  with one or more invalid lint codes.

# Test Plan

Updated fixtures and snapshots

Fixes #22429, #21873


---

_Comment by @astral-sh-bot[bot] on 2026-01-16 00:44_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `rule` added by @amyreese on 2026-01-16 00:50_

---

_Label `suppression` added by @amyreese on 2026-01-16 00:50_

---

_Review requested from @MichaReiser by @amyreese on 2026-01-16 00:50_

---

_Review requested from @ntBre by @amyreese on 2026-01-16 00:50_

---

_@MichaReiser reviewed on 2026-01-16 08:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:462 on 2026-01-16 08:24_

Nice

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:416 on 2026-01-16 08:28_

The suppression range here is a bit misleading because it suggests that all codes are unused. I think I would either:

* Highlight the entire suppression diagnostic. That makes it clear that something's up with the suppression and not with an individual code
* Keep underlining the codes, but only group adjacent codes (in which case we'd emit two diagnostics in this case). I slightly prefer this as it gives more precise ranges. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:235 on 2026-01-16 08:34_

Is my understanding correct that the suppressions in `self.valid` are sorted by their `TextRange`? If so, you could then avoid using a hash map and instead use a single `Option<(TextRange, SuppressionDiagnostic)>` that you "flush" when the `TextRange` no longer matches `suppression.comments.disable_comment().range` (and when you're done processing all `self.valid` 

---

_@MichaReiser approved on 2026-01-16 08:36_

---
