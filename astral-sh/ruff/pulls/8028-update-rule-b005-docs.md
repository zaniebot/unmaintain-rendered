```yaml
number: 8028
title: Update rule B005 docs
type: pull_request
state: merged
author: ahmedabdou14
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/fix_rule_b005_example
created_at: 2023-10-17T21:49:24Z
updated_at: 2023-10-17T22:39:07Z
url: https://github.com/astral-sh/ruff/pull/8028
synced_at: 2026-01-12T02:32:41Z
```

# Update rule B005 docs

---

_Pull request opened by @ahmedabdou14 on 2023-10-17 21:49_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Rule B005 of flake8-bugbear docs has a typo in one of the examples that leads to a confusion in the correctness of `.strip()`  method

![image](https://github.com/astral-sh/ruff/assets/104530599/b4e19751-558e-4ebb-b82f-25c321ddc32b)

```python
# Wrong output (used in docs) 
"text.txt".strip(".txt")  # "ex" 

# Correct output
"text.txt".strip(".txt")  # "e"
```


---

_Comment by @github-actions[bot] on 2023-10-17 22:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Renamed from "Update strip_with_multi_characters.rs doc" to "Update rule B005 docs" by @ahmedabdou14 on 2023-10-17 22:06_

---

_Label `documentation` added by @charliermarsh on 2023-10-17 22:32_

---

_Merged by @charliermarsh on 2023-10-17 22:32_

---

_Closed by @charliermarsh on 2023-10-17 22:32_

---

_Comment by @charliermarsh on 2023-10-17 22:32_

Thank you!

---

_Branch deleted on 2023-10-17 22:39_

---
