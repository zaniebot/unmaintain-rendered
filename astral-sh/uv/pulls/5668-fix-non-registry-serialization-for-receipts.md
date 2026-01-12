```yaml
number: 5668
title: Fix non-registry serialization for receipts
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/ser
created_at: 2024-07-31T19:27:16Z
updated_at: 2024-07-31T21:09:55Z
url: https://github.com/astral-sh/uv/pull/5668
synced_at: 2026-01-12T16:06:57Z
```

# Fix non-registry serialization for receipts

---

_@charliermarsh_

## Summary

Fixes a bug in #5494. The `RequirementSourceWire` representation was ambiguous, and so the order of the fields meant that all variants were mapped to `Registry` when deserializing. (So the snapshots were right, but behaviors were wrong.)


---

_Label `bug` added by @charliermarsh on 2024-07-31 19:27_

---

_Label `preview` added by @charliermarsh on 2024-07-31 19:27_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:324 on 2024-07-31 19:28_

?

---

_@zanieb reviewed on 2024-07-31 19:28_

---

_@zanieb approved on 2024-07-31 19:29_

---

_@charliermarsh reviewed on 2024-07-31 19:30_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:324 on 2024-07-31 19:30_

Needed to disambiguate the tests when running locally (otherwise this test is a prefix of another), fixed.

---

_Merged by @charliermarsh on 2024-07-31 21:09_

---

_Closed by @charliermarsh on 2024-07-31 21:09_

---

_Branch deleted on 2024-07-31 21:09_

---
