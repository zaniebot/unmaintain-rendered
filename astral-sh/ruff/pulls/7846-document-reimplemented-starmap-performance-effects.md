```yaml
number: 7846
title: "Document `reimplemented-starmap` performance effects"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: starmap-docs
created_at: 2023-10-07T10:03:32Z
updated_at: 2024-03-21T07:22:02Z
url: https://github.com/astral-sh/ruff/pull/7846
synced_at: 2026-01-12T15:55:25Z
```

# Document `reimplemented-starmap` performance effects

---

_@tjkuson_

## Summary

Document the performance effects of `itertools.starmap`, including that it is actually slower than comprehensions in Python 3.12.

Closes #7771.

## Test Plan

`python scripts/check_docs_formatted.py`


---

_Marked ready for review by @tjkuson on 2023-10-07 10:15_

---

_Comment by @github-actions[bot] on 2023-10-07 10:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh approved on 2023-10-07 13:26_

---

_Label `documentation` added by @charliermarsh on 2023-10-07 13:27_

---

_Merged by @charliermarsh on 2023-10-07 13:27_

---

_Closed by @charliermarsh on 2023-10-07 13:27_

---

_Branch deleted on 2024-03-21 07:22_

---
