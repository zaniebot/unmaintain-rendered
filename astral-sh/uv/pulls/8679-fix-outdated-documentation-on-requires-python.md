```yaml
number: 8679
title: "Fix outdated documentation on `Requires-Python`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/universal-strategy
created_at: 2024-10-29T18:59:48Z
updated_at: 2024-10-29T20:18:45Z
url: https://github.com/astral-sh/uv/pull/8679
synced_at: 2026-01-12T16:08:26Z
```

# Fix outdated documentation on `Requires-Python`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/8675.


---

_Review requested from @konstin by @charliermarsh on 2024-10-29 18:59_

---

_Label `documentation` added by @charliermarsh on 2024-10-29 18:59_

---

_Review comment by @konstin on `crates/uv-resolver/src/requires_python.rs`:26 on 2024-10-29 19:08_

That isn't the union, but the intersection, isn't it?

---

_@konstin reviewed on 2024-10-29 19:10_

---

_@charliermarsh reviewed on 2024-10-29 20:03_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:26 on 2024-10-29 20:03_

Yes, good catch.

---

_@charliermarsh reviewed on 2024-10-29 20:09_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:26 on 2024-10-29 20:09_

```suggestion
    /// For a workspace, it's the intersection of all `requires-python` values in the workspace. If no
```

---

_Merged by @charliermarsh on 2024-10-29 20:18_

---

_Closed by @charliermarsh on 2024-10-29 20:18_

---

_Branch deleted on 2024-10-29 20:18_

---
