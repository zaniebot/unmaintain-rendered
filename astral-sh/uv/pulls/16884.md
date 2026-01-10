```yaml
number: 16884
title: "Use `UV_WORKING_DIR` for consistency"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/working-dir
created_at: 2025-11-28T16:59:58Z
updated_at: 2025-11-30T15:59:06Z
url: https://github.com/astral-sh/uv/pull/16884
synced_at: 2026-01-10T05:49:14Z
```

# Use `UV_WORKING_DIR` for consistency

---

_Pull request opened by @charliermarsh on 2025-11-28 16:59_

## Summary

Closes https://github.com/astral-sh/uv/issues/16870.


---

_Label `cli` added by @charliermarsh on 2025-11-28 17:00_

---

_Review requested from @zanieb by @charliermarsh on 2025-11-28 17:00_

---

_Review requested from @konstin by @charliermarsh on 2025-11-28 17:00_

---

_Marked ready for review by @charliermarsh on 2025-11-28 17:00_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:1129 on 2025-11-30 15:16_

Technically you should use this placeholder and we update it on release

```suggestion
    #[attr_added_in("next version")]
```

---

_@zanieb reviewed on 2025-11-30 15:16_

---

_@zanieb approved on 2025-11-30 15:16_

---

_@charliermarsh reviewed on 2025-11-30 15:46_

---

_Review comment by @charliermarsh on `crates/uv-static/src/env_vars.rs`:1129 on 2025-11-30 15:46_

TIL thanks!

---

_Merged by @charliermarsh on 2025-11-30 15:59_

---

_Closed by @charliermarsh on 2025-11-30 15:59_

---

_Branch deleted on 2025-11-30 15:59_

---
