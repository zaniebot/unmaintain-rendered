```yaml
number: 4438
title: Fix expected-indentation errors with end-of-line comments
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/indentation
created_at: 2023-05-15T13:23:31Z
updated_at: 2023-05-16T14:45:56Z
url: https://github.com/astral-sh/ruff/pull/4438
synced_at: 2026-01-12T15:55:15Z
```

# Fix expected-indentation errors with end-of-line comments

---

_@charliermarsh_

See: #4418 (which GitHub closed due to a misunderstood merge).

---

_Comment by @github-actions[bot] on 2023-05-15 13:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:104 on 2023-05-15 15:50_

Do we still need the extra casing after the changes in RustPython land?

---

_@MichaReiser reviewed on 2023-05-15 15:50_

---

_@charliermarsh reviewed on 2023-05-15 16:06_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:104 on 2023-05-15 16:06_

We definitely need _some_ change, I _think_ we want to just remove comments entirely here since end-of-line is now always indicated by a newline token?

---

_Merged by @charliermarsh on 2023-05-16 14:45_

---

_Closed by @charliermarsh on 2023-05-16 14:45_

---

_Branch deleted on 2023-05-16 14:45_

---
