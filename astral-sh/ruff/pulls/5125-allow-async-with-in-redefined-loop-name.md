```yaml
number: 5125
title: "Allow `async with` in `redefined-loop-name`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/async
created_at: 2023-06-15T18:46:41Z
updated_at: 2023-06-15T19:00:20Z
url: https://github.com/astral-sh/ruff/pull/5125
synced_at: 2026-01-12T15:55:17Z
```

# Allow `async with` in `redefined-loop-name`

---

_@charliermarsh_

## Summary

Closes #5124.


---

_Label `bug` added by @charliermarsh on 2023-06-15 18:46_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1526 on 2023-06-15 18:46_

Changed this while I was here.

---

_@charliermarsh reviewed on 2023-06-15 18:46_

---

_@charliermarsh reviewed on 2023-06-15 18:47_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/redefined_loop_name.rs`:16 on 2023-06-15 18:47_

Made these private, moved them below the violation (so the violation appears first in the module).

---

_@charliermarsh reviewed on 2023-06-15 18:47_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/redefined_loop_name.rs`:258 on 2023-06-15 18:47_

We don't need these generics, and we can remove some of the lifetimes too (unrelated to the bug fix).

---

_Review requested from @konstin by @charliermarsh on 2023-06-15 18:47_

---

_@konstin approved on 2023-06-15 18:55_

that was quick, thanks!

---

_Merged by @charliermarsh on 2023-06-15 19:00_

---

_Closed by @charliermarsh on 2023-06-15 19:00_

---

_Branch deleted on 2023-06-15 19:00_

---
