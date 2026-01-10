```yaml
number: 10841
title: Error when workspace contains conflicting Python requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/conflict
created_at: 2025-01-22T02:46:09Z
updated_at: 2025-01-22T17:22:54Z
url: https://github.com/astral-sh/uv/pull/10841
synced_at: 2026-01-10T11:45:14Z
```

# Error when workspace contains conflicting Python requirements

---

_Pull request opened by @charliermarsh on 2025-01-22 02:46_

## Summary

If members define disjoint Python requirements, we should error. Right now, it seems that it maps to unbounded and leads to weird behavior.

Closes https://github.com/astral-sh/uv/issues/10835.


---

_Review requested from @zanieb by @charliermarsh on 2025-01-22 02:46_

---

_Review requested from @konstin by @charliermarsh on 2025-01-22 02:46_

---

_Label `error messages` added by @charliermarsh on 2025-01-22 02:46_

---

_@charliermarsh reviewed on 2025-01-22 02:46_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/init.rs`:506 on 2025-01-22 02:46_

This is kind of gross, but struggling to improve it.

---

_@charliermarsh reviewed on 2025-01-22 02:46_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:340 on 2025-01-22 02:46_

This just includes all of the specifiers.

---

_@zanieb approved on 2025-01-22 03:15_

---

_@zanieb reviewed on 2025-01-22 03:16_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:5289 on 2025-01-22 03:16_

It'd be ideal to include sources here, if feasible. Perhaps as a future change.

Maybe like

```
error: The workspace contains conflicting Python requirements:
    - project: `>=3.12`
    - child: `==3.10`
```



---

_@konstin approved on 2025-01-22 12:35_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:5289 on 2025-01-22 17:06_

I think I'll fix it here in this PR, just annoying to do in a `thiserror` macro.

---

_@charliermarsh reviewed on 2025-01-22 17:06_

---

_Merged by @charliermarsh on 2025-01-22 17:22_

---

_Closed by @charliermarsh on 2025-01-22 17:22_

---

_Branch deleted on 2025-01-22 17:22_

---
