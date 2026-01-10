```yaml
number: 4341
title: "Refactor `Toolchain` API to always take `ToolchainRequest` instead of `str`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/toolchain-find-request
created_at: 2024-06-16T15:24:26Z
updated_at: 2024-06-17T16:16:16Z
url: https://github.com/astral-sh/uv/pull/4341
synced_at: 2026-01-10T13:54:02Z
```

# Refactor `Toolchain` API to always take `ToolchainRequest` instead of `str`

---

_Pull request opened by @zanieb on 2024-06-16 15:24_

This API was taking an `Option<&str>` for caller convenience in some places but we ought to just take a `ToolchainRequest` consistently.

---

_Label `internal` added by @zanieb on 2024-06-16 15:24_

---

_@konstin approved on 2024-06-17 08:58_

---

_Merged by @zanieb on 2024-06-17 16:16_

---

_Closed by @zanieb on 2024-06-17 16:16_

---

_Branch deleted on 2024-06-17 16:16_

---
