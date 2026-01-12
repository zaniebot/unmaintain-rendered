```yaml
number: 3248
title: Fix compression tests in QEMU cross-compilation environments
type: pull_request
state: merged
author: OctopusET
labels: []
assignees: []
merged: true
base: master
head: fix-qemu-tests
created_at: 2025-12-17T16:29:27Z
updated_at: 2025-12-17T16:39:23Z
url: https://github.com/BurntSushi/ripgrep/pull/3248
synced_at: 2026-01-12T18:23:15Z
```

# Fix compression tests in QEMU cross-compilation environments

---

_@OctopusET_

## Problem
Compression tests (lz4, brotli, zstd) fail on `riscv64` when running under QEMU user-mode emulation. The tests attempt to execute compression commands even when the required tools are missing, instead of skipping as expected.

## Root Cause
This is caused by a known limitation in QEMU user-mode's `posix_spawn` implementation. When spawning a non-existent command:
- **Expected:** `posix_spawn` returns `ENOENT` (Error).
- **QEMU:** `posix_spawn` returns `0` (Success), but the child process silently fails with exit code `127`.

**Technical Details:**
1. glibc 2.34+ attempts `clone3()`, which QEMU rejects with `ENOSYS`.
2. It falls back to `clone(CLONE_VM | CLONE_VFORK | ...)`.
3. QEMU silently strips `CLONE_VM` because it cannot share address spaces in process-based emulation ([syscall.c:6729-6731](https://gitlab.com/qemu-project/qemu/-/blob/master/linux-user/syscall.c#L6729-6731)).
4. Consequently, `fork()` is used (Copy-on-Write).
5. When the child writes the error code (`errno`) to the memory, it writes to its *own* copy. The parent reads the original memory (where error is 0) and assumes success.

This issue specifically affects newer environments (glibc 2.34+, e.g., Ubuntu 22.04+). Older environments like Ubuntu 20.04 (glibc 2.31) are unaffected because they handle `vfork` fallbacks differently, masking the QEMU bug.

## Why this fix is needed
Currently, `aarch64` tests pass only because the CI uses Ubuntu 20.04 (glibc 2.31). Once `cross-rs` or CI images update to Ubuntu 22.04+ (glibc 2.35+), `aarch64` and other architectures will start failing too. This PR prevents future breakages across all architectures.

## The Fix
Updated `cmd_exists()` to verify that the command **executed successfully** (exit code 0), rather than just checking if the spawn call itself returned `Ok`.

* **Native / Normal:** Returns `Err(NotFound)` -> function returns `false`.
* **QEMU (Bugged):** Returns `Ok(ExitStatus(127))` -> `status.success()` is `false` -> function returns `false`.

I also removed the `#[cfg(not(target_arch = "riscv64"))]` guards from the compression tests as they are no longer needed.

## Verification

Verified on an x86_64 host cross-compiling to `riscv64` and `aarch64` (via QEMU user-mode). Tests now correctly skip when compression tools are missing, and pass when they are present.

## Reference

* Related Rust issue: https://github.com/rust-lang/rust/issues/90825

---

_@BurntSushi approved on 2025-12-17 16:37_

Wow, amazing analysis! Thank you. Seems like it was quite gnarly to debug.

---

_Merged by @BurntSushi on 2025-12-17 16:38_

---

_Closed by @BurntSushi on 2025-12-17 16:38_

---

_Comment by @OctopusET on 2025-12-17 16:39_

I couldn't understand why only risc-v tests are failing. Turns out it was QEMU+glibc issue.

---
