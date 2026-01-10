```yaml
number: 10234
title: Add support for Python interpreters on ARMv5TE platforms
type: pull_request
state: merged
author: lscorcia
labels:
  - compatibility
assignees: []
merged: true
base: main
head: issue-10157
created_at: 2024-12-30T07:51:03Z
updated_at: 2025-09-26T13:14:48Z
url: https://github.com/astral-sh/uv/pull/10234
synced_at: 2026-01-10T06:36:15Z
```

# Add support for Python interpreters on ARMv5TE platforms

---

_Pull request opened by @lscorcia on 2024-12-30 07:51_

## Summary
Allows uv to recognize the ARMv5TE platform. This platform is currently supported on Debian distributions. It is an older 32 bit platform mostly used in embedded devices, currently in rust tier 2.5 so it requires cross compilation.

Fixes #10157 .

## Test Plan
Tested directly on device by applying a slightly different patch to tag 0.5.4 which is used by the current Home Assistant version (2024.12.5). After the patch Home Assistant is able to recognize the Python venv and setup its dependencies.

Patched uv was built with 
```
$ CARGO_TARGET_ARMV5TE_UNKNOWN_LINUX_GNUEABI_LINKER="/usr/bin/arm-linux-gnueabi-gcc" maturin build --release --target armv5te-unknown-linux-gnueabi --manylinux off
``` 

The target wheel was then moved on the device and installed via pip install.

---

_Converted to draft by @lscorcia on 2024-12-30 08:09_

---

_Marked ready for review by @lscorcia on 2024-12-30 08:37_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:292 on 2024-12-30 15:33_

We need to remove this part -- Warehouse doesn't allow this tag.

---

_@charliermarsh reviewed on 2024-12-30 15:33_

---

_@charliermarsh reviewed on 2024-12-30 15:34_

---

_Review comment by @charliermarsh on `Cargo.toml`:300 on 2024-12-30 15:34_

We should remove this. This would only be relevant if we were building uv for this platform.

---

_@charliermarsh reviewed on 2024-12-30 15:44_

---

_Review comment by @charliermarsh on `crates/uv-python/python/packaging/_manylinux.py`:61 on 2024-12-30 15:44_

I'm not certain that this should be changed. I don't know if armv5te has a compatible ABI.

---

_@charliermarsh reviewed on 2024-12-30 15:46_

---

_Review comment by @charliermarsh on `crates/uv-python/python/packaging/_manylinux.py`:61 on 2024-12-30 15:46_

I think this probably _shouldn't_ be changed, since this code is vendored and it's not present in pip etc.

Can you revert this part and re-test?

---

_Label `compatibility` added by @charliermarsh on 2024-12-30 15:52_

---

_@lscorcia reviewed on 2024-12-30 16:29_

---

_Review comment by @lscorcia on `crates/uv-python/python/packaging/_manylinux.py`:61 on 2024-12-30 16:29_

You are right, I did not check. armv5te does not support manylinux. Reverted and tested on device, uv works fine.

---

_Merged by @charliermarsh on 2024-12-30 16:49_

---

_Closed by @charliermarsh on 2024-12-30 16:49_

---

_Comment by @charliermarsh on 2024-12-30 16:50_

Thanks!

---

_Renamed from "Initial support for ARMv5TE platform via cross compilation" to "Add support for Python interpreters on ARMv5TE platforms" by @zanieb on 2024-12-30 17:15_

---

_Branch deleted on 2025-09-26 13:14_

---
