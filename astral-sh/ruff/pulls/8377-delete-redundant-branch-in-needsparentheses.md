```yaml
number: 8377
title: "Delete redundant branch in `NeedsParentheses`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: delete-dead-branch
created_at: 2023-10-31T11:49:55Z
updated_at: 2023-10-31T12:06:18Z
url: https://github.com/astral-sh/ruff/pull/8377
synced_at: 2026-01-12T15:55:26Z
```

# Delete redundant branch in `NeedsParentheses`

---

_@MichaReiser_


## Summary

Delete an unnecessary branch in the `NeedsParentheses` implementation of `ExprAttribute`. 

The branch isn't required because `ExprName::needs_parentheses` always returns `BestFit`

## Test Plan

`cargo test`, The similarity index remains unchanged


---

_Comment by @MichaReiser on 2023-10-31 11:50_

Current dependencies on/for this PR:
* main
  * **PR #8377** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8377" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8377?utm_source=stack-comment).

---

_Label `internal` added by @MichaReiser on 2023-10-31 11:50_

---

_Comment by @github-actions[bot] on 2023-10-31 12:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no format changes.



---

_Merged by @MichaReiser on 2023-10-31 12:06_

---

_Closed by @MichaReiser on 2023-10-31 12:06_

---

_Branch deleted on 2023-10-31 12:06_

---
