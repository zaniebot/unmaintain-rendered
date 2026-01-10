```yaml
number: 4055
title: "Reduce visibility of `lowering`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lower
created_at: 2024-06-05T17:32:27Z
updated_at: 2024-06-05T17:39:38Z
url: https://github.com/astral-sh/uv/pull/4055
synced_at: 2026-01-10T13:54:02Z
```

# Reduce visibility of `lowering`

---

_Pull request opened by @charliermarsh on 2024-06-05 17:32_

## Summary

This makes `lowering.rs` internal to the metadata package.


---

_@charliermarsh reviewed on 2024-06-05 17:32_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/mod.rs`:401 on 2024-06-05 17:32_

(Just moved from `lowering.rs` because it accesses a private API on `Metadata`.)

---

_Label `internal` added by @charliermarsh on 2024-06-05 17:32_

---

_Marked ready for review by @charliermarsh on 2024-06-05 17:33_

---

_Merged by @charliermarsh on 2024-06-05 17:39_

---

_Closed by @charliermarsh on 2024-06-05 17:39_

---

_Branch deleted on 2024-06-05 17:39_

---
