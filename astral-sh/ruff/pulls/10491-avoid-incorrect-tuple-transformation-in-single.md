```yaml
number: 10491
title: "Avoid incorrect tuple transformation in single-element case (`C409`)"
type: pull_request
state: merged
author: WindowGenerator
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-unnecessary-literal-within-tuple-call
created_at: 2024-03-20T16:29:00Z
updated_at: 2024-03-22T07:11:16Z
url: https://github.com/astral-sh/ruff/pull/10491
synced_at: 2026-01-10T22:47:02Z
```

# Avoid incorrect tuple transformation in single-element case (`C409`)

---

_Pull request opened by @WindowGenerator on 2024-03-20 16:29_

# Summary
Fixed: incorrect rule transformation rule C409 with single element.
# Test Plan
Added examples from #10323 to test fixtures.

---

_Comment by @github-actions[bot] on 2024-03-20 16:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-03-20 17:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_literal_within_tuple_call.rs`:109 on 2024-03-20 17:24_

This is going to lose the spacing around the element. Can you take a look at how we solved this in https://github.com/astral-sh/ruff/pull/10461?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_literal_within_tuple_call.rs`:109 on 2024-03-20 17:24_

For example: https://github.com/astral-sh/ruff/pull/10461/files#diff-8c56901e841c4e8733b61b77c68b174ea49dc98d02221715acafc8cfff273971R734

---

_@charliermarsh reviewed on 2024-03-20 17:24_

---

_@WindowGenerator reviewed on 2024-03-20 19:35_

---

_Review comment by @WindowGenerator on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_literal_within_tuple_call.rs`:109 on 2024-03-20 19:35_

Thank you for suggestion! 

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-20 20:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-20 20:28_

---

_Label `bug` added by @charliermarsh on 2024-03-20 20:28_

---

_@charliermarsh approved on 2024-03-21 00:01_

Thanks!

---

_Renamed from "Fixed: incorrect rule transformation rule C409 with single element" to "Avoid incorrect tuple transformation in single-element case (`C409`)" by @charliermarsh on 2024-03-21 00:01_

---

_Merged by @charliermarsh on 2024-03-21 00:09_

---

_Closed by @charliermarsh on 2024-03-21 00:09_

---

_Branch deleted on 2024-03-22 07:11_

---
