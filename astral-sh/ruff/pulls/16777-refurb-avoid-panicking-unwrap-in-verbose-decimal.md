```yaml
number: 16777
title: "[`refurb`] Avoid panicking `unwrap` in `verbose-decimal-constructor` (`FURB157`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: decimal-unwrap
created_at: 2025-03-16T15:39:23Z
updated_at: 2025-03-17T10:09:08Z
url: https://github.com/astral-sh/ruff/pull/16777
synced_at: 2026-01-12T15:55:57Z
```

# [`refurb`] Avoid panicking `unwrap` in `verbose-decimal-constructor` (`FURB157`)

---

_@dylwil3_

Removes `find`/`unwrap` in creating fix for [verbose-decimal-constructor (FURB157)](https://docs.astral.sh/ruff/rules/verbose-decimal-constructor/#verbose-decimal-constructor-furb157).

Previously, we tried to maintain trivia such as whitespace, implicitly concatenated strings, etc. when replacing something that parses to the value `"-nan"`. This requires too much complexity when the user has unicode escape sequences such as `"\N{character tabulation}\N{hyphen-minus}nan"`. So in this case we just replace the string literal with exactly `"nan"`. 

Closes #16771 


---

_Label `bug` added by @dylwil3 on 2025-03-16 15:39_

---

_Label `fixes` added by @dylwil3 on 2025-03-16 15:39_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/refurb/FURB157.py`:70 on 2025-03-16 16:36_

Could you add a `Decimal(float("-nAn"))` case here too, just to show that the fix will also remove custom casing?

---

_@InSyncWithFoo reviewed on 2025-03-16 16:36_

---

_Closed by @MichaReiser on 2025-03-17 07:51_

---

_Reopened by @MichaReiser on 2025-03-17 07:51_

---

_@MichaReiser approved on 2025-03-17 07:54_

---

_Comment by @github-actions[bot] on 2025-03-17 08:06_

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

_Merged by @dylwil3 on 2025-03-17 10:09_

---

_Closed by @dylwil3 on 2025-03-17 10:09_

---
