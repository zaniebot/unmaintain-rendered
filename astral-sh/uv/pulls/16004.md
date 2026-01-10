```yaml
number: 16004
title: "deps: bump astral-tokio-tar to 0.5.5"
type: pull_request
state: merged
author: woodruffw
labels:
  - no-build
assignees: []
merged: true
base: main
head: ww/bump-tokio-tar
created_at: 2025-09-23T16:31:53Z
updated_at: 2025-09-23T17:46:10Z
url: https://github.com/astral-sh/uv/pull/16004
synced_at: 2026-01-10T06:36:15Z
```

# deps: bump astral-tokio-tar to 0.5.5

---

_Pull request opened by @woodruffw on 2025-09-23 16:31_

## Summary

For https://github.com/astral-sh/tokio-tar/security/advisories/GHSA-3wgq-wrwc-vqmv.

## Test Plan

No functional changes expected, but we'll see what happens in CI.

---

_Review requested from @zanieb by @woodruffw on 2025-09-23 16:31_

---

_Assigned to @woodruffw by @woodruffw on 2025-09-23 16:31_

---

_@zanieb approved on 2025-09-23 16:44_

---

_Comment by @woodruffw on 2025-09-23 16:45_

Ah, there's a defect in the release -- it has some aggressive logging in it. I'm going to fix that with another patch release.

---

_Renamed from "deps: bump astral-tokio-tar to 0.5.4" to "deps: bump astral-tokio-tar to 0.5.5" by @woodruffw on 2025-09-23 16:52_

---

_@zanieb approved on 2025-09-23 17:01_

---

_@woodruffw reviewed on 2025-09-23 17:03_

---

_Review comment by @woodruffw on `crates/uv/src/commands/build_frontend.rs`:402 on 2025-09-23 17:03_

Note: both errors are still possible here, but we probably really only expect the first (absent an intentionally malicious embedded venv). I see @konstin has already hit the pain of insufficient error typing here 😅 

---

_Label `no-build` added by @woodruffw on 2025-09-23 17:27_

---

_Merged by @woodruffw on 2025-09-23 17:46_

---

_Closed by @woodruffw on 2025-09-23 17:46_

---

_Branch deleted on 2025-09-23 17:46_

---
