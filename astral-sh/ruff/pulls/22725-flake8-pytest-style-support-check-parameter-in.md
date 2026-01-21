```yaml
number: 22725
title: "[`flake8-pytest-style`] Support `check` parameter in `PT011`"
type: pull_request
state: merged
author: harupy
labels:
  - rule
assignees: []
merged: true
base: main
head: pt011-support-check-parameter
created_at: 2026-01-19T15:07:10Z
updated_at: 2026-01-21T08:25:22Z
url: https://github.com/astral-sh/ruff/pull/22725
synced_at: 2026-01-21T09:03:02Z
```

# [`flake8-pytest-style`] Support `check` parameter in `PT011`

---

_@harupy_

## Summary

PT011 (`pytest-raises-too-broad`) now recognizes the `check` parameter introduced in pytest 8.4.0.

Closes #22673

## Test plan

Added test cases for `check` parameter (valid callback and `check=None`).


---

_Comment by @astral-sh-bot[bot] on 2026-01-19 16:05_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review requested from @ntBre by @ntBre on 2026-01-20 15:16_

---

_@amyreese approved on 2026-01-21 01:01_

---

_Label `rule` added by @MichaReiser on 2026-01-21 08:21_

---

_@MichaReiser approved on 2026-01-21 08:22_

---

_Comment by @MichaReiser on 2026-01-21 08:22_

Thank you

---

_Merged by @MichaReiser on 2026-01-21 08:22_

---

_Closed by @MichaReiser on 2026-01-21 08:22_

---

_Branch deleted on 2026-01-21 08:25_

---
