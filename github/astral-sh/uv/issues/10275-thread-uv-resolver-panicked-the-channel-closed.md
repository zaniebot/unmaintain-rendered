---
number: 10275
title: "thread 'uv-resolver' panicked - The channel closed unexpectedly"
type: issue
state: closed
author: gaborbernat
labels:
  - bug
assignees: []
created_at: 2025-01-02T16:30:37Z
updated_at: 2025-01-02T17:01:43Z
url: https://github.com/astral-sh/uv/issues/10275
synced_at: 2026-01-07T13:12:18-06:00
---

# thread 'uv-resolver' panicked - The channel closed unexpectedly

---

_Issue opened by @gaborbernat on 2025-01-02 16:30_

```
[2025-01-02T05:12:56.696Z] Using Python 3.11.11 environment at: .tox/dev
[2025-01-02T05:12:56.696Z] thread 'uv-resolver' panicked at crates/uv-resolver/src/resolver/environment.rs:568:9:
[2025-01-02T05:12:56.696Z] resolver must be in universal mode for forking
[2025-01-02T05:12:56.696Z] note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
[2025-01-02T05:12:56.696Z] error: The channel closed unexpectedly
```

Is happening in Docker (Linux) only,  somehow on macOS does not happen. Started with `0.5.12`.

---

_Referenced in [astral-sh/uv#10274](../../astral-sh/uv/pulls/10274.md) on 2025-01-02 16:30_

---

_Label `bug` added by @charliermarsh on 2025-01-02 16:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-02 16:32_

---

_Comment by @gaborbernat on 2025-01-02 16:34_

Likely introduced by https://github.com/astral-sh/uv/pull/10046 👍 

---

_Closed by @charliermarsh on 2025-01-02 16:40_

---

_Closed by @charliermarsh on 2025-01-02 16:40_

---

_Comment by @gaborbernat on 2025-01-02 17:01_

Built the PR, and can confirm that it works:
```
  dev: OK (160.36=setup[159.09]+cmd[1.15,0.12] seconds)
```

---
