---
number: 14421
title: "`lock::lock_pytorch_cpu` times out on Windows"
type: issue
state: open
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2025-07-02T13:29:54Z
updated_at: 2025-07-02T13:31:07Z
url: https://github.com/astral-sh/uv/issues/14421
synced_at: 2026-01-07T13:12:18-06:00
---

# `lock::lock_pytorch_cpu` times out on Windows

---

_Issue opened by @zanieb on 2025-07-02 13:29_

```
     TIMEOUT [  90.070s] uv::it lock::lock_pytorch_cpu
  stdout ───

    running 1 test
    test lock::lock_pytorch_cpu has been running for over 60 seconds
```

---

_Label `ci-flake` added by @zanieb on 2025-07-02 13:29_

---

_Comment by @zanieb on 2025-07-02 13:31_

Hoping https://github.com/astral-sh/uv/pull/14170 helps

---
