```yaml
number: 10060
title: Detect literals with unary operators (UP018)
type: pull_request
state: merged
author: sanxiyn
labels:
  - bug
assignees: []
merged: true
base: main
head: UP018-fix
created_at: 2024-02-20T12:01:10Z
updated_at: 2024-02-21T02:49:54Z
url: https://github.com/astral-sh/ruff/pull/10060
synced_at: 2026-01-10T22:57:10Z
```

# Detect literals with unary operators (UP018)

---

_Pull request opened by @sanxiyn on 2024-02-20 12:01_

Fix #10029.

---

_Label `bug` added by @charliermarsh on 2024-02-20 16:03_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-20 16:03_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs`:234 on 2024-02-20 17:29_

Would you mind adding a comment why this check is necessary for `unary` only and add a test case that verifies that ruff early returns if it's an unary and neither an `Int` nor `Float`

---

_@MichaReiser approved on 2024-02-20 17:30_

---

_@charliermarsh reviewed on 2024-02-20 18:11_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs`:234 on 2024-02-20 18:11_

I moved this up into the match.

---

_Merged by @charliermarsh on 2024-02-20 18:21_

---

_Closed by @charliermarsh on 2024-02-20 18:21_

---

_Comment by @github-actions[bot] on 2024-02-20 18:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Branch deleted on 2024-02-21 02:49_

---
