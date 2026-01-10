```yaml
number: 17274
title: Track retry counts originating from early middleware errors
type: pull_request
state: merged
author: EliteTK
labels:
  - bug
assignees: []
merged: true
base: main
head: tk/dont-drop-retries
created_at: 2025-12-30T17:58:30Z
updated_at: 2026-01-05T20:06:41Z
url: https://github.com/astral-sh/uv/pull/17274
synced_at: 2026-01-10T05:49:14Z
```

# Track retry counts originating from early middleware errors

---

_Pull request opened by @EliteTK on 2025-12-30 17:58_

## Summary

Fixes #17266.

The retry count was getting dropped by `ErrorKind::from_retry_middleware` and `<Error as From<ErrorKind>>::from` so we were doing more retries than we should have.

## Test Plan

Added a testcase for the specific error path in the issue. Added an expect to the other retry test too.

---

_Review requested from @konstin by @EliteTK on 2025-12-30 17:58_

---

_Label `bug` added by @EliteTK on 2025-12-30 17:58_

---

_@zanieb approved on 2025-12-30 21:01_

This seems more appropriate than https://github.com/astral-sh/uv/pull/17267

---

_Comment by @EliteTK on 2025-12-30 21:03_

@zanieb do you think its worth waiting on @konstin?

---

_Renamed from "Track retry counts originating from middleware errors" to "Track retry counts originating from early middleware errors" by @EliteTK on 2025-12-30 21:04_

---

_Review comment by @konstin on `crates/uv/tests/it/network.rs`:467 on 2026-01-05 17:46_

Can you move this to a helper too, with a doc comment about which error path this checks?

---

_@konstin approved on 2026-01-05 17:46_

---

_Merged by @EliteTK on 2026-01-05 20:06_

---

_Closed by @EliteTK on 2026-01-05 20:06_

---

_Branch deleted on 2026-01-05 20:06_

---
