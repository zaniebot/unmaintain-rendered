```yaml
number: 2930
title: Add filtering of patch Python versions unless explicitly requested
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/patch-filter
created_at: 2024-04-09T13:32:06Z
updated_at: 2024-04-09T15:04:28Z
url: https://github.com/astral-sh/uv/pull/2930
synced_at: 2026-01-12T16:05:19Z
```

# Add filtering of patch Python versions unless explicitly requested

---

_@zanieb_

Elides Python patch versions from the test suite unless the test specifically requests a patch version.

This reduces some toil when not using our bootstrapped Python versions.

Partially addresses https://github.com/astral-sh/uv/issues/2165 though we'll need changes to the scenario tests to really support their case.

---

_Label `internal` added by @zanieb on 2024-04-09 13:32_

---

_Label `testing` added by @zanieb on 2024-04-09 13:32_

---

_Review requested from @konstin by @zanieb on 2024-04-09 13:44_

---

_Review requested from @charliermarsh by @zanieb on 2024-04-09 13:44_

---

_Review comment by @konstin on `crates/uv/tests/common/mod.rs`:130 on 2024-04-09 14:10_

Can you add a comment why we do this?

---

_@konstin approved on 2024-04-09 14:11_

---

_@zanieb reviewed on 2024-04-09 14:25_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:130 on 2024-04-09 14:25_

Yep!

---

_Merged by @zanieb on 2024-04-09 15:04_

---

_Closed by @zanieb on 2024-04-09 15:04_

---

_Branch deleted on 2024-04-09 15:04_

---
