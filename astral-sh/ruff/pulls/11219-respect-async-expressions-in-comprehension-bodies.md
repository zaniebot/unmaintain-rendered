```yaml
number: 11219
title: "Respect `async` expressions in comprehension bodies"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/comp
created_at: 2024-04-30T18:31:12Z
updated_at: 2024-04-30T18:43:40Z
url: https://github.com/astral-sh/ruff/pull/11219
synced_at: 2026-01-12T15:55:37Z
```

# Respect `async` expressions in comprehension bodies

---

_@charliermarsh_

## Summary

We weren't recursing into the comprehension body.

---

_Label `bug` added by @charliermarsh on 2024-04-30 18:31_

---

_@charliermarsh reviewed on 2024-04-30 18:31_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/unused_async.rs`:86 on 2024-04-30 18:31_

(No change to this method, just reordered to match trait definition.)

---

_@charliermarsh reviewed on 2024-04-30 18:31_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/unused_async.rs`:91 on 2024-04-30 18:31_

This is the fix.

---

_Marked ready for review by @charliermarsh on 2024-04-30 18:31_

---

_Merged by @charliermarsh on 2024-04-30 18:38_

---

_Closed by @charliermarsh on 2024-04-30 18:38_

---

_Branch deleted on 2024-04-30 18:38_

---

_Comment by @github-actions[bot] on 2024-04-30 18:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
