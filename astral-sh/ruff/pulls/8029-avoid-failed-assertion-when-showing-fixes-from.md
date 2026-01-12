```yaml
number: 8029
title: Avoid failed assertion when showing fixes from stdin
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/fix-map
created_at: 2023-10-17T23:41:01Z
updated_at: 2023-10-18T01:50:41Z
url: https://github.com/astral-sh/ruff/pull/8029
synced_at: 2026-01-12T02:32:41Z
```

# Avoid failed assertion when showing fixes from stdin

---

_Pull request opened by @charliermarsh on 2023-10-17 23:41_

## Summary

When linting, we store a map from file path to fixes, which we then use to show a fix summary in the printer.

In the printer, we assume that if the map is non-empty, then we have at least one fix. But this isn't enforced by the fix struct, since you can have an entry from (file path) to (empty fix table). In practice, this only bites us when linting from `stdin`, since when linting across multiple files, we have an `AddAssign` on `Diagnostics` that avoids adding empty entries to the map. When linting from `stdin`, we create the map directly, and so it _is_ possible to have a non-empty map that doesn't contain any fixes, leading to a panic.

This PR introduces a dedicated struct to make these constraints part of the formal interface.

Closes https://github.com/astral-sh/ruff/issues/8027.

## Test Plan

`cargo test` (notice two failures are removed)


---

_Label `bug` added by @charliermarsh on 2023-10-17 23:41_

---

_Label `cli` added by @charliermarsh on 2023-10-17 23:41_

---

_@charliermarsh reviewed on 2023-10-17 23:49_

---

_Review comment by @charliermarsh on `crates/ruff_cli/tests/integration_test.rs`:1280 on 2023-10-17 23:49_

I think this is correct, and was wrong before.

---

_Review requested from @zanieb by @charliermarsh on 2023-10-17 23:49_

---

_Comment by @github-actions[bot] on 2023-10-17 23:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:162 on 2023-10-18 00:54_

This is a somewhat expensive check for large fix tables. Can we instead avoid adding entries when they have no fixes or would that be confusing because it is no longer guaranteed that a key is present after inserting it.

---

_@MichaReiser reviewed on 2023-10-18 00:55_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/integration_test.rs`:1280 on 2023-10-18 00:58_

Agreed

---

_@zanieb reviewed on 2023-10-18 00:58_

---

_@charliermarsh reviewed on 2023-10-18 01:17_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:162 on 2023-10-18 01:17_

We can... It puts us back in the world we were in before (we'll need to take more care when creating these). I was hoping to capture hide that detail from callers. But I can change it.

---

_@MichaReiser reviewed on 2023-10-18 01:19_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:162 on 2023-10-18 01:19_

Can't we handle it inside of `from_iter`? 

---

_@charliermarsh reviewed on 2023-10-18 01:26_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:162 on 2023-10-18 01:26_

Ah, right, I moved it.

---

_@MichaReiser approved on 2023-10-18 01:44_

---

_Merged by @charliermarsh on 2023-10-18 01:50_

---

_Closed by @charliermarsh on 2023-10-18 01:50_

---

_Branch deleted on 2023-10-18 01:50_

---
