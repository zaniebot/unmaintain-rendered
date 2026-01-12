```yaml
number: 3230
title: Unroll self-dependencies via extras
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - performance
assignees: []
merged: true
base: main
head: charlie/priority
created_at: 2024-04-24T01:22:41Z
updated_at: 2024-04-24T11:51:57Z
url: https://github.com/astral-sh/uv/pull/3230
synced_at: 2026-01-12T16:05:30Z
```

# Unroll self-dependencies via extras

---

_@charliermarsh_

## Summary

We now recursively expand any self-dependencies via extras, which lets us detect conflicts sooner and avoid building unnecessary versions of packages that are excluded via the extra.

Closes https://github.com/astral-sh/uv/issues/3135.



---

_Label `bug` added by @charliermarsh on 2024-04-24 01:22_

---

_Label `performance` added by @charliermarsh on 2024-04-24 01:22_

---

_@charliermarsh reviewed on 2024-04-24 01:23_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:4006 on 2024-04-24 01:23_

This test fails if you try to build `extras==0.0.2`.

---

_Marked ready for review by @charliermarsh on 2024-04-24 01:23_

---

_@konstin approved on 2024-04-24 11:32_

---

_Merged by @charliermarsh on 2024-04-24 11:51_

---

_Closed by @charliermarsh on 2024-04-24 11:51_

---

_Branch deleted on 2024-04-24 11:51_

---
