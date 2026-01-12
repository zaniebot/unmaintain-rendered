```yaml
number: 6161
title: "add initial set of \"workflow\" tests"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/test-workflow
created_at: 2024-08-16T18:17:44Z
updated_at: 2024-08-19T13:11:41Z
url: https://github.com/astral-sh/uv/pull/6161
synced_at: 2026-01-12T16:07:15Z
```

# add initial set of "workflow" tests

---

_@BurntSushi_

This PR adds a new initial set of workflow tests. A "workflow" test is a test that performs multiple actions with `uv` and then asserts something about the final state. In the current set of tests, we are only interested in changes (or absence of changes) to the lock file.

This does include one test that asserts a wrong result. I filed a bug for that at #6158.

Reviewers should go commit-by-commit here.

---

_Review requested from @zanieb by @BurntSushi on 2024-08-16 18:21_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-08-16 18:21_

---

_@charliermarsh reviewed on 2024-08-16 20:01_

Interesting, the diff approach is very clever.

---

_Review comment by @konstin on `crates/uv/tests/workflow.rs`:35 on 2024-08-19 12:26_

Do we use those filters?

---

_Merged by @BurntSushi on 2024-08-19 12:33_

---

_Closed by @BurntSushi on 2024-08-19 12:33_

---

_Branch deleted on 2024-08-19 12:33_

---

_@konstin approved on 2024-08-19 12:37_

---

_Review comment by @BurntSushi on `crates/uv/tests/workflow.rs`:35 on 2024-08-19 13:11_

We use these filters when snapshotting lock files and this is snapshotting a diff of a lock file, so my thinking is that it makes sense to use them here too.

---

_@BurntSushi reviewed on 2024-08-19 13:11_

---
