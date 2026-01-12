```yaml
number: 11528
title: Add missing functions to NumPy 2.0 migration rule
type: pull_request
state: merged
author: mtsokol
labels:
  - rule
assignees: []
merged: true
base: main
head: npy201-alltrue
created_at: 2024-05-24T09:34:21Z
updated_at: 2024-05-27T10:04:01Z
url: https://github.com/astral-sh/ruff/pull/11528
synced_at: 2026-01-12T15:55:38Z
```

# Add missing functions to NumPy 2.0 migration rule

---

_@mtsokol_

Hi! 

I left out some of the functions in the migration rule which became removed in NumPy 2.0:
- `np.alltrue`
- `np.anytrue`
- `np.cumproduct`
- `np.product`

Addressing: https://github.com/numpy/numpy/issues/26493

---

_Renamed from "Add `np.alltrue` to NumPy 2.0 migration rule" to "Add missing functions to NumPy 2.0 migration rule" by @mtsokol on 2024-05-24 09:53_

---

_Comment by @github-actions[bot] on 2024-05-24 10:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-05-26 17:24_

Perfect, thanks!

---

_Label `rule` added by @charliermarsh on 2024-05-26 17:24_

---

_Merged by @charliermarsh on 2024-05-26 17:24_

---

_Closed by @charliermarsh on 2024-05-26 17:24_

---

_Branch deleted on 2024-05-27 10:04_

---
