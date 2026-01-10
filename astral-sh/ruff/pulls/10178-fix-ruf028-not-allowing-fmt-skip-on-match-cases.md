```yaml
number: 10178
title: "Fix RUF028 not allowing `# fmt: skip` on match cases"
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
assignees: []
merged: true
base: main
head: jane/linter/ruf028/match-case-header
created_at: 2024-03-01T05:56:53Z
updated_at: 2024-03-01T08:36:24Z
url: https://github.com/astral-sh/ruff/pull/10178
synced_at: 2026-01-10T22:47:01Z
```

# Fix RUF028 not allowing `# fmt: skip` on match cases

---

_Pull request opened by @snowsignal on 2024-03-01 05:56_

## Summary

Fixes #10174 by allowing match cases to be enclosing nodes for suppression comments. `else/elif` clauses are now also allowed to be enclosing nodes.

## Test Plan
I've added the offending code from the original issue to the `RUF028` snapshot test, and I've also expanded it to test the allowed `else/elif` clause.


---

_Label `bug` added by @snowsignal on 2024-03-01 05:56_

---

_Review requested from @MichaReiser by @snowsignal on 2024-03-01 05:56_

---

_Comment by @github-actions[bot] on 2024-03-01 06:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-03-01 07:10_

---

_Merged by @snowsignal on 2024-03-01 08:36_

---

_Closed by @snowsignal on 2024-03-01 08:36_

---

_Branch deleted on 2024-03-01 08:36_

---
