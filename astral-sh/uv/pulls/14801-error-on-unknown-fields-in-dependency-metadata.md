```yaml
number: 14801
title: "Error on unknown fields in `dependency-metadata`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/req
created_at: 2025-07-21T21:57:18Z
updated_at: 2025-07-21T22:15:04Z
url: https://github.com/astral-sh/uv/pull/14801
synced_at: 2026-01-10T06:53:02Z
```

# Error on unknown fields in `dependency-metadata`

---

_Pull request opened by @charliermarsh on 2025-07-21 21:57_

## Summary

Closes https://github.com/astral-sh/uv/issues/14800.


---

_Label `error messages` added by @charliermarsh on 2025-07-21 21:57_

---

_Marked ready for review by @charliermarsh on 2025-07-21 21:57_

---

_@charliermarsh reviewed on 2025-07-21 21:59_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/dependency_metadata.rs`:47 on 2025-07-21 21:59_

This was duplicated.

---

_@charliermarsh reviewed on 2025-07-21 21:59_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/dependency_metadata.rs`:42 on 2025-07-21 21:59_

If the first `find` matches, _both_ `inspect` closures run, so we logged `Found global metadata...` incorrectly.

---

_Merged by @charliermarsh on 2025-07-21 22:15_

---

_Closed by @charliermarsh on 2025-07-21 22:15_

---

_Branch deleted on 2025-07-21 22:15_

---
