```yaml
number: 13817
title: "Debug `sync_dry_run` flake by panicking with verbose output on failure"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/debug-sync_dry_run
created_at: 2025-06-03T14:17:58Z
updated_at: 2025-06-12T13:05:28Z
url: https://github.com/astral-sh/uv/pull/13817
synced_at: 2026-01-10T11:10:42Z
```

# Debug `sync_dry_run` flake by panicking with verbose output on failure

---

_Pull request opened by @zanieb on 2025-06-03 14:17_

Investigating #13744 

I tried reproducing here by running the test in a loop, but could not. I presume it's an interaction with other tests.

This drops the snapshot, but I think it's worth it to try to examine the flake?

---

_Renamed from "Debug: Run `sync_dry_run` on infinite loop" to "Debug `sync_dry_run` flake by panicking with verbose output on failure" by @zanieb on 2025-06-11 13:51_

---

_Marked ready for review by @zanieb on 2025-06-11 13:56_

---

_Label `testing` added by @zanieb on 2025-06-11 13:56_

---

_@Gankra approved on 2025-06-12 02:32_

---

_Merged by @zanieb on 2025-06-12 13:05_

---

_Closed by @zanieb on 2025-06-12 13:05_

---

_Branch deleted on 2025-06-12 13:05_

---
