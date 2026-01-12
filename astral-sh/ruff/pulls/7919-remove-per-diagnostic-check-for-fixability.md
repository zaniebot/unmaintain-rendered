```yaml
number: 7919
title: Remove per-diagnostic check for fixability
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/fix
created_at: 2023-10-11T14:26:15Z
updated_at: 2023-10-11T16:16:04Z
url: https://github.com/astral-sh/ruff/pull/7919
synced_at: 2026-01-12T02:32:41Z
```

# Remove per-diagnostic check for fixability

---

_Pull request opened by @charliermarsh on 2023-10-11 14:26_

## Summary

Throughout the codebase, we have this pattern:

```rust
let mut diagnostic = ...
if checker.patch(Rule::UnusedVariable) {
    // Do the fix.
}
diagnostics.push(diagnostic)
```

This was helpful when we computed fixes lazily; however, we now compute fixes eagerly, and this is _only_ used to ensure that we don't generate fixes for rules marked as unfixable.

We often forget to add this, and it leads to bugs in enforcing `--unfixable`.

This PR instead removes all of these checks, moving the responsibility of enforcing `--unfixable` up to `check_path`. This is similar to how @zanieb handled the `--extend-unsafe` logic: we post-process the diagnostics to remove any fixes that should be ignored.


---

_Review requested from @zanieb by @charliermarsh on 2023-10-11 14:26_

---

_Review requested from @konstin by @charliermarsh on 2023-10-11 14:26_

---

_@charliermarsh reviewed on 2023-10-11 14:26_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/linter.rs`:268 on 2023-10-11 14:26_

This is the actual code change.

---

_@charliermarsh reviewed on 2023-10-11 14:26_

This is a five line change, which then requires removing those `if` guards in nearly every rule.

---

_Label `internal` added by @charliermarsh on 2023-10-11 14:27_

---

_@zanieb approved on 2023-10-11 14:55_

---

_@konstin approved on 2023-10-11 15:49_

---

_Merged by @charliermarsh on 2023-10-11 16:09_

---

_Closed by @charliermarsh on 2023-10-11 16:09_

---

_Branch deleted on 2023-10-11 16:09_

---

_Comment by @github-actions[bot] on 2023-10-11 16:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
