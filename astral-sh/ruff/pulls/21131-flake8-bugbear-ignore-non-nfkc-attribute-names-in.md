```yaml
number: 21131
title: "[`flake8-bugbear`] Ignore non-NFKC attribute names in B009 and B010 (`B009`, `B010`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-21126
created_at: 2025-10-29T22:19:36Z
updated_at: 2025-11-03T15:07:41Z
url: https://github.com/astral-sh/ruff/pull/21131
synced_at: 2026-01-12T15:57:17Z
```

# [`flake8-bugbear`] Ignore non-NFKC attribute names in B009 and B010 (`B009`, `B010`)

---

_@danparizher_

## Summary

This PR updates the `B009` (`get-attr-with-constant`) and `B010` (`set-attr-with-constant`) rules to ignore non-NFKC attribute names, preventing refactoring suggestions that could change program behavior.

Fixes #21126

## Problem Analysis

Python normalizes identifiers using NFKC (Normalization Form KC) normalization. When using attribute access syntax (e.g., `obj.attr`), Python automatically normalizes the identifier. However, when using `getattr` or `setattr` with a string literal, the identifier is not normalized.

The issue occurs when an attribute name contains characters that normalize differently under NFKC. For example:
- The long s character `"ſ"` normalizes to `"s"` in NFKC
- Using `setattr(ns, "ſ", 1)` creates a distinct attribute from `ns.s`
- If Ruff suggests replacing `setattr(ns, "ſ", 1)` with `ns.ſ = 1`, Python would normalize `ns.ſ` to `ns.s`, which changes the program's behavior

The bug was that Ruff's B009 and B010 rules didn't account for this normalization difference, potentially suggesting unsafe refactorings.

## Approach

The fix adds a check in both `B009` and `B010` rule implementations to detect non-NFKC attribute names. Before suggesting a refactoring, the code now:

1. Normalizes the attribute name using NFKC
2. Compares the normalized form with the original
3. If they differ, the rule skips the check (returns early) to avoid suggesting an unsafe refactoring

This ensures that:
- Rules only suggest refactorings when it's safe (i.e., when NFKC normalization wouldn't change the attribute name)
- The fix is minimal and doesn't affect other rule functionality
- Edge cases are handled correctly (non-NFKC names are silently ignored)

Test cases were added to verify that non-NFKC attribute names (like `"ſ"`) are correctly ignored by both rules.

---

_Comment by @github-actions[bot] on 2025-10-29 22:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-10-30 01:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:75 on 2025-10-30 01:02_

Let's move this after line 78 as this comparison is more expensive than testing if it is the builtin getattr

---

_@MichaReiser reviewed on 2025-10-30 01:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B009_B010.py`:75 on 2025-10-30 01:04_

I suggest including a short explanation **why** they should be ignored

---

_Review requested from @MichaReiser by @danparizher on 2025-10-30 01:35_

---

_@MichaReiser reviewed on 2025-10-30 13:01_

I'm leaning towards marking the fix as unsafe if the attribute is different after nfkc normalization instead of omitting the diagnostic entirely as I doubt that it's very intentional. 

The counter-argument to this is that someone might deliberately use `getattr` because they're aware of the difference and it matters to them. The synta then acts as an explicit expression of their intent and forcing them to add a noqa comment feels redundant. 

I'm leaning towards doing the former because the second seems extremely rare and a noqa suppression (with an explanation why) can serve as additional documentation for future readers. But I'm curious to hear what others think

---

_Comment by @amyreese on 2025-10-31 00:47_

+1 to just marking it unsafe

---

_@MichaReiser reviewed on 2025-11-01 22:54_

Thank you. This looks good. The only thing left to do is to update the rule documentation with an explanation when the fix is marked as unsafe

---

_@MichaReiser approved on 2025-11-03 14:44_

---

_Label `bug` added by @MichaReiser on 2025-11-03 14:45_

---

_Label `fixes` added by @MichaReiser on 2025-11-03 14:45_

---

_Merged by @MichaReiser on 2025-11-03 14:45_

---

_Closed by @MichaReiser on 2025-11-03 14:45_

---

_Branch deleted on 2025-11-03 15:07_

---
