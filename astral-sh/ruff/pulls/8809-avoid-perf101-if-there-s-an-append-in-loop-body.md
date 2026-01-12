```yaml
number: 8809
title: "Avoid `PERF101` if there's an append in loop body"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/perf101
created_at: 2023-11-21T16:00:19Z
updated_at: 2023-11-21T21:35:43Z
url: https://github.com/astral-sh/ruff/pull/8809
synced_at: 2026-01-10T23:40:55Z
```

# Avoid `PERF101` if there's an append in loop body

---

_Pull request opened by @dhruvmanila on 2023-11-21 16:00_

## Summary

Avoid `PERF101` if there's an append in loop body

## Test Plan

Add new test cases for this pattern.

fixes: #8746


---

_Comment by @dhruvmanila on 2023-11-21 16:00_

Current dependencies on/for this PR:
* main
  * **PR #8809** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8809?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8809?utm_source=stack-comment).

---

_Label `bug` added by @dhruvmanila on 2023-11-21 16:00_

---

_Review requested from @zanieb by @dhruvmanila on 2023-11-21 16:00_

---

_@zanieb reviewed on 2023-11-21 16:11_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/perflint/PERF101.py`:55 on 2023-11-21 16:11_

Can you add a test case that appends to a variable with a different name?

---

_Comment by @github-actions[bot] on 2023-11-21 16:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @zanieb by @dhruvmanila on 2023-11-21 20:03_

---

_@zanieb approved on 2023-11-21 21:24_

---

_Merged by @dhruvmanila on 2023-11-21 21:35_

---

_Closed by @dhruvmanila on 2023-11-21 21:35_

---

_Branch deleted on 2023-11-21 21:35_

---
