```yaml
number: 7037
title: "Allow multiple packages for `uv tool upgrade/uninstall`"
type: pull_request
state: merged
author: blueraft
labels:
  - cli
assignees: []
merged: true
base: main
head: uv-tool-multiple-pkg
created_at: 2024-09-04T17:20:47Z
updated_at: 2024-09-06T08:03:46Z
url: https://github.com/astral-sh/uv/pull/7037
synced_at: 2026-01-12T16:07:38Z
```

# Allow multiple packages for `uv tool upgrade/uninstall`

---

_@blueraft_

## Summary

Resolves https://github.com/astral-sh/uv/issues/6571

## Test Plan

`cargo test`


---

_Label `cli` added by @zanieb on 2024-09-04 17:22_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:3124 on 2024-09-04 20:14_

Can these be `Vec<PackageName>`? I think there's no semantic difference between `None` and `Some(vec![])` in this case.

---

_@charliermarsh reviewed on 2024-09-04 20:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-04 20:44_

---

_@blueraft reviewed on 2024-09-04 21:07_

---

_Review comment by @blueraft on `crates/uv-cli/src/lib.rs`:3124 on 2024-09-04 21:07_

that's true, I've changed it to `Vec<PackageName>`

---

_Merged by @charliermarsh on 2024-09-04 21:18_

---

_Closed by @charliermarsh on 2024-09-04 21:18_

---

_Comment by @charliermarsh on 2024-09-04 21:18_

Looks great, thank you!

---

_@charliermarsh reviewed on 2024-09-04 21:19_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:3124 on 2024-09-04 21:19_

Can we do the same for uninstall?

---

_@blueraft reviewed on 2024-09-05 11:03_

---

_Review comment by @blueraft on `crates/uv-cli/src/lib.rs`:3124 on 2024-09-05 11:03_

Created a follow up PR: https://github.com/astral-sh/uv/pull/7077

---

_Branch deleted on 2024-09-06 08:03_

---
