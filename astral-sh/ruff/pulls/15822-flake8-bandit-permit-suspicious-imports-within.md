```yaml
number: 15822
title: "[`flake8-bandit`] Permit suspicious imports within stub files (`S4`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: s4-ignore-stubs
created_at: 2025-01-30T01:24:35Z
updated_at: 2025-01-30T16:48:37Z
url: https://github.com/astral-sh/ruff/pull/15822
synced_at: 2026-01-12T15:55:52Z
```

# [`flake8-bandit`] Permit suspicious imports within stub files (`S4`)

---

_@tjkuson_

## Summary

Permits suspicious imports (the `S4` namespaced diagnostics) from stub files.

Closes #15207.

## Test Plan

Added tests and ran `cargo nextest run`. The test files are copied from the `.py` variants.

---

_Comment by @github-actions[bot] on 2025-01-30 01:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-01-30 05:40_

LGTM! Thank you!

---

_Label `rule` added by @dylwil3 on 2025-01-30 05:41_

---

_Label `preview` added by @dylwil3 on 2025-01-30 05:41_

---

_Renamed from "[`flake8-bandit`] Permit suspicious imports within stub files" to "[`flake8-bandit`] Permit suspicious imports within stub files (`S4`)" by @dylwil3 on 2025-01-30 05:42_

---

_Merged by @dylwil3 on 2025-01-30 05:42_

---

_Closed by @dylwil3 on 2025-01-30 05:42_

---

_Branch deleted on 2025-01-30 16:48_

---
