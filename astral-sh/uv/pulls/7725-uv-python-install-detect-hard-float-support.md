```yaml
number: 7725
title: "uv python install: Detect hard-float support"
type: pull_request
state: closed
author: kakkoyun
labels:
  - bug
  - uv python
assignees: []
base: main
head: detect_hardfloat
created_at: 2024-09-26T19:35:14Z
updated_at: 2024-10-29T23:33:21Z
url: https://github.com/astral-sh/uv/pull/7725
synced_at: 2026-01-10T12:53:54Z
```

# uv python install: Detect hard-float support

---

_Pull request opened by @kakkoyun on 2024-09-26 19:35_

Signed-off-by: Kemal Akkoyun <kakkoyun@gmail.com>

## Summary

This pull request fixes [#6873](https://github.com/astral-sh/uv/issues/6873) by implementing
a mechanism to detect whether the ARM architecture supports the VFP (Vector Floating Point) hardware,
which is critical for selecting the appropriate `libc` variant.

The detection is based on reading the flags from `/proc/cpuinfo`.
Specifically, if the `vfp` flag is present, the system will use the `gnueabihf` (hard-float ABI) variant;
if not, it will default to `gnueabi` (soft-float ABI).

For additional context, see the [Debian documentation on the ARM Hard Float Port](https://wiki.debian.org/ArmHardFloatPort#VFP).

## Test Plan

- [ ] Verify that the correct `libc` variant is selected on an ARM system with and without VFP support.


---

_Review requested from @konstin by @kakkoyun on 2024-09-26 19:35_

---

_Review requested from @zanieb by @kakkoyun on 2024-09-26 19:35_

---

_@charliermarsh reviewed on 2024-09-26 19:38_

---

_Review comment by @charliermarsh on `Cargo.lock`:2736 on 2024-09-26 19:38_

Can we disable the optional `chrono` feature?

---

_Converted to draft by @kakkoyun on 2024-09-26 19:43_

---

_Assigned to @konstin by @zanieb on 2024-09-26 20:33_

---

_Review comment by @konstin on `crates/uv-python/src/cpuinfo.rs`:22 on 2024-09-27 09:47_

Can you link the debian docs page here?

---

_Review comment by @konstin on `crates/uv-python/src/cpuinfo.rs`:15 on 2024-09-27 09:49_

The docstring should mention something about arm, afaik this instruction set does not exist on x86

---

_@konstin approved on 2024-09-27 09:49_

Looks good!

---

_@kakkoyun reviewed on 2024-09-27 11:54_

---

_Review comment by @kakkoyun on `crates/uv-python/src/cpuinfo.rs`:15 on 2024-09-27 11:54_

That's true. I will add the documentation.

---

_Marked ready for review by @zanieb on 2024-10-01 18:54_

---

_Label `bug` added by @zanieb on 2024-10-01 18:55_

---

_Label `uv python` added by @zanieb on 2024-10-01 18:55_

---

_Comment by @kakkoyun on 2024-10-02 15:47_

Quick update: I'm still trying to set up some old RPIs to test this.

---

_Comment by @zanieb on 2024-10-29 23:33_

Replacing with https://github.com/astral-sh/uv/pull/8498

---

_Closed by @zanieb on 2024-10-29 23:33_

---
