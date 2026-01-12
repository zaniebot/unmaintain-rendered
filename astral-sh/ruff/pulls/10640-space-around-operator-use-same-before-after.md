```yaml
number: 10640
title: "space_around_operator: use same before/after numbers"
type: pull_request
state: merged
author: pjonsson
labels:
  - documentation
assignees: []
merged: true
base: main
head: e242-docs
created_at: 2024-03-27T22:51:06Z
updated_at: 2024-03-28T07:37:10Z
url: https://github.com/astral-sh/ruff/pull/10640
synced_at: 2026-01-12T15:55:32Z
```

# space_around_operator: use same before/after numbers

---

_@pjonsson_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The example for tab-after-comma (E242):
```python
a = 4,\t5
```
Use instead:
```python
a = 4, 3
```
is confusing since both the whitespace and the numbers are changed.

Change so the examples use the same numbers before/after.

## Test Plan

Untested.


---

_Comment by @github-actions[bot] on 2024-03-27 23:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-03-27 23:31_

Thanks, not sure how this happened!

---

_Label `documentation` added by @charliermarsh on 2024-03-27 23:31_

---

_Merged by @charliermarsh on 2024-03-27 23:31_

---

_Closed by @charliermarsh on 2024-03-27 23:31_

---

_Branch deleted on 2024-03-28 07:37_

---
