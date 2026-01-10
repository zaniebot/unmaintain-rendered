```yaml
number: 11720
title: Upgrade Rust toolchain to 1.85
type: pull_request
state: merged
author: samypr100
labels:
  - internal
assignees: []
merged: true
base: main
head: rust1.85
created_at: 2025-02-23T00:17:58Z
updated_at: 2025-03-03T21:40:02Z
url: https://github.com/astral-sh/uv/pull/11720
synced_at: 2026-01-10T11:10:38Z
```

# Upgrade Rust toolchain to 1.85

---

_Pull request opened by @samypr100 on 2025-02-23 00:17_

## Summary

* Upgrade the rust toolchain to 1.85.0. This does not increase the MSRV.
* Update windows trampoline to 1.86 nightly beta (previously in 1.85 nightly beta).

## Test Plan

Existing tests


---

_Marked ready for review by @samypr100 on 2025-02-23 00:27_

---

_@konstin approved on 2025-02-23 15:51_

---

_Comment by @konstin on 2025-02-23 15:52_

I won't update the trampoline binaries, I'll leave that to the next time we have code changes in the trampoline.

---

_Merged by @konstin on 2025-02-23 15:52_

---

_Closed by @konstin on 2025-02-23 15:52_

---

_Label `internal` added by @konstin on 2025-02-23 15:57_

---

_Branch deleted on 2025-02-23 18:04_

---

_Comment by @zanieb on 2025-03-03 21:39_

I wonder if this is causing the libatomic failure in https://github.com/astral-sh/uv/actions/runs/13640597809/job/38129388713

---

_Comment by @zanieb on 2025-03-03 21:40_

Nevermind this went out in 0.6.3

---
