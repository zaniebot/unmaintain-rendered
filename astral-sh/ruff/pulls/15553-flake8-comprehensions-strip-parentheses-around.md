```yaml
number: 15553
title: "[`flake8-comprehensions`] strip parentheses around generators in `unnecessary-generator-set` (`C401`)"
type: pull_request
state: merged
author: wooly18
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: unnecessary-generator-set-autofix-parentheses
created_at: 2025-01-17T16:05:20Z
updated_at: 2025-01-17T17:15:11Z
url: https://github.com/astral-sh/ruff/pull/15553
synced_at: 2026-01-12T15:55:51Z
```

# [`flake8-comprehensions`] strip parentheses around generators in `unnecessary-generator-set` (`C401`)

---

_@wooly18_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes parentheses not being stripped in C401. Pretty much the same as #11607 which fixed it for C400.

## Test Plan
`cargo nextest run`


---

_Comment by @github-actions[bot] on 2025-01-17 16:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[flake8-comprehensions] strip parentheses around generators in C401" to "[`flake8-comprehensions`] strip parentheses around generators in `unnecessary-generator-set` (`C401`)" by @dylwil3 on 2025-01-17 16:19_

---

_Label `rule` added by @dylwil3 on 2025-01-17 16:20_

---

_Label `fixes` added by @dylwil3 on 2025-01-17 16:20_

---

_Label `rule` removed by @dylwil3 on 2025-01-17 16:20_

---

_Label `bug` added by @dylwil3 on 2025-01-17 16:21_

---

_@MichaReiser approved on 2025-01-17 17:08_

---

_Comment by @MichaReiser on 2025-01-17 17:08_

Thanks

---

_Merged by @MichaReiser on 2025-01-17 17:08_

---

_Closed by @MichaReiser on 2025-01-17 17:08_

---

_Branch deleted on 2025-01-17 17:15_

---
