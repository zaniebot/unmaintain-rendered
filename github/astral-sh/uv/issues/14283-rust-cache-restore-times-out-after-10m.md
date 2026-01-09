---
number: 14283
title: Rust cache restore times out after 10m
type: issue
state: open
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2025-06-26T16:29:57Z
updated_at: 2025-06-26T16:30:23Z
url: https://github.com/astral-sh/uv/issues/14283
synced_at: 2026-01-07T13:12:18-06:00
---

# Rust cache restore times out after 10m

---

_Issue opened by @zanieb on 2025-06-26 16:29_

e.g., https://github.com/astral-sh/uv/actions/runs/15906868894/job/44864007951

```
... Restoring cache ...
Cache hit for: v0-rust-cargo-build-msrv-Linux-x64-ce6c74b5-89a9b164
Received 33554432 of 532334052 (6.3%), 32.0 MBs/sec
Received 167772160 of 532334052 (31.5%), 80.0 MBs/sec
Received 264241152 of 532334052 (49.6%), 84.0 MBs/sec
Error: The operation was canceled.
```

---

_Label `ci-flake` added by @zanieb on 2025-06-26 16:29_

---

_Referenced in [Swatinem/rust-cache#248](../../Swatinem/rust-cache/issues/248.md) on 2025-06-26 18:27_

---
