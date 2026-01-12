```yaml
number: 8541
title: Update applicability messages for clarity in tests
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/app-msg
created_at: 2023-11-07T15:26:59Z
updated_at: 2023-11-07T16:27:59Z
url: https://github.com/astral-sh/ruff/pull/8541
synced_at: 2026-01-10T23:40:55Z
```

# Update applicability messages for clarity in tests

---

_Pull request opened by @zanieb on 2023-11-07 15:26_

These names are only ever displayed internally right now and we could be clearer in our test snapshots.

The diff is kind of scary because all of the tests fixtures are updated.

---

_Label `internal` added by @zanieb on 2023-11-07 15:26_

---

_Comment by @zanieb on 2023-11-07 15:32_

This is disruptive so I've been putting it off but might be worth it.

---

_@charliermarsh reviewed on 2023-11-07 15:35_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/message/diff.rs`:59 on 2023-11-07 15:35_

Nit: "Display-only fix"? As-is it reads as imperative to me, like an action.

---

_Review comment by @zanieb on `crates/ruff_linter/src/message/diff.rs`:59 on 2023-11-07 15:40_

That works. Should we consider improving the name of that applicability level?

---

_@zanieb reviewed on 2023-11-07 15:40_

---

_Comment by @github-actions[bot] on 2023-11-07 15:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2023-11-07 15:44_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/message/diff.rs`:59 on 2023-11-07 15:44_

To `DisplayOnly`?

---

_@charliermarsh reviewed on 2023-11-07 15:45_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/message/diff.rs`:59 on 2023-11-07 15:45_

I'm fine with that

---

_@charliermarsh approved on 2023-11-07 16:02_

---

_Merged by @zanieb on 2023-11-07 16:11_

---

_Closed by @zanieb on 2023-11-07 16:11_

---

_Branch deleted on 2023-11-07 16:11_

---
