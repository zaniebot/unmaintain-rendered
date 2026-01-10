---
number: 17016
title: Increase the size of binary build runners
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
base: main
head: zb/release-runners-2
created_at: 2025-12-06T14:28:37Z
updated_at: 2025-12-09T22:04:46Z
url: https://github.com/astral-sh/uv/pull/17016
synced_at: 2026-01-10T01:26:19Z
---

# Increase the size of binary build runners

---

_Pull request opened by @zanieb on 2025-12-06 14:28_

Summary from asking Claude to compare the runtime

```
  | Job                       | Before | After | Saved | Reduction |
  |---------------------------|--------|-------|-------|-----------|
  | macos-aarch64             | 19.1m  | 9.8m  | 9.2m  | 48%       |
  | macos-x86_64              | 15.1m  | 9.2m  | 5.9m  | 39%       |
  | linux-arm (aarch64)       | 18.6m  | 12.2m | 6.4m  | 34%       |
  | linux-arm (armv7)         | 17.4m  | 11.5m | 5.9m  | 34%       |
  | musllinux-cross (aarch64) | 19.3m  | 13.2m | 6.1m  | 32%       |
  | linux-arm (arm)           | 16.6m  | 11.4m | 5.2m  | 31%       |
  | musllinux-cross (armv7)   | 16.4m  | 10.8m | 5.6m  | 34%       |
```
The longest-running affected job went from 19.3m → 13.2m (saved 6.1 minutes, 32% faster).

---

_Comment by @zanieb on 2025-12-08 12:18_

This is currently blocked on the Depot Windows runners missing Clang.

---

_Marked ready for review by @zanieb on 2025-12-09 21:52_

---

_Merged by @zanieb on 2025-12-09 22:04_

---

_Closed by @zanieb on 2025-12-09 22:04_

---

_Branch deleted on 2025-12-09 22:04_

---
