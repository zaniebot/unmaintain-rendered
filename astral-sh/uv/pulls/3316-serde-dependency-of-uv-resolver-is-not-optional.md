```yaml
number: 3316
title: "`serde` dependency of `uv-resolver` is not optional"
type: pull_request
state: closed
author: ibraheemdev
labels:
  - internal
assignees: []
base: main
head: uv-resolver-serde
created_at: 2024-04-29T19:03:04Z
updated_at: 2024-04-29T19:50:27Z
url: https://github.com/astral-sh/uv/pull/3316
synced_at: 2026-01-10T14:37:54Z
```

# `serde` dependency of `uv-resolver` is not optional

---

_Pull request opened by @ibraheemdev on 2024-04-29 19:03_

## Summary

https://github.com/astral-sh/uv/pull/3314 makes `uv-resolver` unconditionally depend on `serde`, so the dependency should not be optional. This was breaking https://github.com/astral-sh/uv/pull/3281.

---

_Label `internal` added by @ibraheemdev on 2024-04-29 19:04_

---

_@zanieb approved on 2024-04-29 19:13_

---

_Comment by @ibraheemdev on 2024-04-29 19:33_

Realized the new types don't actually unconditionally required `serde`, so they should only implement `Serialize/Deserialize` if `serde` is enabled.

---

_Closed by @ibraheemdev on 2024-04-29 19:33_

---

_Branch deleted on 2024-04-29 19:50_

---
