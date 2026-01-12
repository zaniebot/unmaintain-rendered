```yaml
number: 15420
title: "extra-build-dependencies: Allow version specifiers if match-runtime is explicitly false"
type: pull_request
state: merged
author: zsol
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: zsol/jj-tplvulzxmuus
created_at: 2025-08-21T15:27:19Z
updated_at: 2025-08-21T16:50:17Z
url: https://github.com/astral-sh/uv/pull/15420
synced_at: 2026-01-12T16:11:44Z
```

# extra-build-dependencies: Allow version specifiers if match-runtime is explicitly false

---

_@zsol_

## Summary

`match-runtime` can be explicitly specified, and if it's `false` it should behave the same way as if it's omitted.

## Test Plan

Added snapshot test

---

_Review requested from @charliermarsh by @konstin on 2025-08-21 16:01_

---

_Review requested from @zanieb by @konstin on 2025-08-21 16:01_

---

_Label `enhancement` added by @konstin on 2025-08-21 16:01_

---

_Label `preview` added by @konstin on 2025-08-21 16:01_

---

_@charliermarsh approved on 2025-08-21 16:50_

Thanks! Good catch.

---

_Merged by @charliermarsh on 2025-08-21 16:50_

---

_Closed by @charliermarsh on 2025-08-21 16:50_

---

_Branch deleted on 2025-08-21 16:50_

---
