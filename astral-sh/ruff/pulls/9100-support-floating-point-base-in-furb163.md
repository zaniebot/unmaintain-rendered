```yaml
number: 9100
title: Support floating-point base in FURB163
type: pull_request
state: merged
author: siiptuo
labels:
  - bug
assignees: []
merged: true
base: main
head: FURB163-float
created_at: 2023-12-11T20:18:31Z
updated_at: 2023-12-11T20:50:53Z
url: https://github.com/astral-sh/ruff/pull/9100
synced_at: 2026-01-12T15:55:27Z
```

# Support floating-point base in FURB163

---

_@siiptuo_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Check floating-point numbers similarly to integers in FURB163. For example, both `math.log(x, 10)` and `math.log(x, 10.0)` should be changed to `math.log10(x)`.

## Test Plan

<!-- How was it tested? -->

Added couple of test cases.

---

_@charliermarsh approved on 2023-12-11 20:30_

Seems reasonable.

---

_Comment by @github-actions[bot] on 2023-12-11 20:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2023-12-11 20:47_

---

_Closed by @charliermarsh on 2023-12-11 20:47_

---

_Label `rule` added by @charliermarsh on 2023-12-11 20:47_

---

_Label `rule` removed by @charliermarsh on 2023-12-11 20:47_

---

_Label `bug` added by @charliermarsh on 2023-12-11 20:47_

---

_Branch deleted on 2023-12-11 20:50_

---
