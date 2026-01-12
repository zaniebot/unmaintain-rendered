```yaml
number: 9034
title: "Avoid `ANN2xx` autofix for abstract methods with empty body"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: dhruv/auto-return
created_at: 2023-12-07T02:35:13Z
updated_at: 2023-12-07T02:47:38Z
url: https://github.com/astral-sh/ruff/pull/9034
synced_at: 2026-01-10T23:40:55Z
```

# Avoid `ANN2xx` autofix for abstract methods with empty body

---

_Pull request opened by @dhruvmanila on 2023-12-07 02:35_

## Summary

This PR updates the `ANN201`, `ANN202`, `ANN205`, and `ANN206` rules to not create a fix for the return type when it's an abstract method and the function body is empty i.e., it only contains either a pass statement, docstring or an ellipsis literal.

fixes: #9004

## Test Plan

Add the following test cases:
- Abstract method with pass statement
- Abstract method with docstring
- Abstract method with ellipsis literal
- Abstract method with possible return type


---

_Comment by @dhruvmanila on 2023-12-07 02:35_

Current dependencies on/for this PR:
* main
  * **PR #9034** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9034?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9034?utm_source=stack-comment).

---

_Comment by @dhruvmanila on 2023-12-07 02:35_

(I hope I understood the issue correctly 😅)

---

_Label `rule` added by @dhruvmanila on 2023-12-07 02:35_

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-12-07 02:35_

---

_@charliermarsh approved on 2023-12-07 02:38_

Looks right to me!

---

_Comment by @github-actions[bot] on 2023-12-07 02:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dhruvmanila on 2023-12-07 02:47_

---

_Closed by @dhruvmanila on 2023-12-07 02:47_

---

_Branch deleted on 2023-12-07 02:47_

---
