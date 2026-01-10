```yaml
number: 16627
title: "[`pylint`] Stabilize `shallow-copy-environ` (`PLW1507`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/plw1507-0.10
created_at: 2025-03-11T14:27:38Z
updated_at: 2025-03-11T15:30:18Z
url: https://github.com/astral-sh/ruff/pull/16627
synced_at: 2026-01-10T19:49:01Z
```

# [`pylint`] Stabilize `shallow-copy-environ` (`PLW1507`)

---

_Pull request opened by @ntBre on 2025-03-11 14:27_

Summary
--

Stabilizes PLW1507. The tests were already in the right place, and I just tidied the docs a little bit.

Test Plan
--

1 issue from 2 weeks ago but just suggesting to mark the fix unsafe. The shallow vs deep copy *does* change the program behavior, just usually in a preferable way.


---

_Label `rule` added by @ntBre on 2025-03-11 14:27_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-11 14:27_

---

_@MichaReiser approved on 2025-03-11 14:28_

---

_Comment by @github-actions[bot] on 2025-03-11 14:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @ntBre on 2025-03-11 15:30_

---

_Closed by @ntBre on 2025-03-11 15:30_

---

_Branch deleted on 2025-03-11 15:30_

---
