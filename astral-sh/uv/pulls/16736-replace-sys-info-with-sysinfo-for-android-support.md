```yaml
number: 16736
title: "Replace `sys-info` with `sysinfo` for Android support; use mimalloc instead of jemalloc on Android"
type: pull_request
state: closed
author: LIghtJUNction
labels: []
assignees: []
draft: true
base: main
head: main
created_at: 2025-11-14T11:16:25Z
updated_at: 2025-11-16T05:16:02Z
url: https://github.com/astral-sh/uv/pull/16736
synced_at: 2026-01-12T16:12:24Z
```

# Replace `sys-info` with `sysinfo` for Android support; use mimalloc instead of jemalloc on Android

---

_@LIghtJUNction_

sys-info: https://crates.io/crates/sys-info
sysinfo: https://crates.io/crates/sysinfo

## Summary

Replace `sys-info` with [sysinfo](https://crates.io/crates/sysinfo) library for better compatibility and Android support, and exclude Android from jemalloc to fix Termux compilation issues.

## Problem

The `sys-info` library is outdated with incomplete documentation and lacks Android support. This causes compatibility issues in modern environments, including Termux on Android.

Additionally, the `tikv-jemalloc-sys` crate fails to compile in Android environments (such as Termux), causing build failures when targeting Android platforms. This is similar to issues previously encountered on FreeBSD, where jemalloc was also excluded.

## Solution

Replaced `sys-info` with the more comprehensive `sysinfo` library, which provides better documentation, active maintenance, and native Android support. This improves cross-platform compatibility while maintaining the same functionality for system information retrieval.

Modified the target condition in Cargo.toml to add `not(target_os = "android")`, ensuring Android falls back to the system default allocator instead of jemalloc. This maintains compatibility while preserving performance optimizations on supported platforms.

## Changes

- Replaced `sys-info` dependency with `sysinfo` across relevant crates (`uv-python`, `uv-client`)
- Updated jemalloc dependency condition to exclude Android
- Follows the same pattern used for FreeBSD exclusion
- Additionally, updated dependency versions and performed build tests. Based on the test feedback, fixed some errors

## Test Plan

1. **Cross-platform compilation**: Verified builds on Windows, Linux, and macOS with the new `sysinfo` library.

2. **Android support testing**:
   - Tested Android platform using cross-compilation:
     ```bash
     cross build --target aarch64-linux-android
     ```
   - Confirmed `sysinfo` library works correctly on Android
   - Termux still has some compilation issues with jemalloc (excluded as fallback)

3. **Linux targets**:
   ```bash
   # x86_64 Linux
   cross build --target x86_64-unknown-linux-gnu


   ```



5. **Functionality verification**: Ensured system information retrieval (OS name, version) works identically with the new library.

6. **Regression testing**: Verified that existing functionality in `uv-python` and `uv-client` crates remains intact.

7. **Dependency update testing**: Validated that updated dependency versions compile correctly and resolve any compatibility issues identified during testing.

8. **Local build verification**: Confirmed successful builds on Windows and local environment.

# WINDOWS(host)
X86:
OK
<img width="731" height="400" alt="图片" src="https://github.com/user-attachments/assets/227755de-8cd3-4f97-9777-03cba227da5e" />

# LINUX(cross)
OK
x86:
<img width="1274" height="802" alt="图片" src="https://github.com/user-attachments/assets/1c550844-0f53-4eef-b64a-f80f6ced67e7" />


# Android(cross)

Ok
aarch64:
<img width="1146" height="1473" alt="图片" src="https://github.com/user-attachments/assets/ddb62609-f2fc-42e2-8a4b-6142a1be0fbf" />


# Android-Termux(host)

Error
Trying to fix...




---

_@zanieb reviewed on 2025-11-14 13:52_

---

_Review comment by @zanieb on `Cargo.toml`:83 on 2025-11-14 13:52_

We shouldn't be bumping the versions of all of our other dependencies here too?

---

_@zanieb reviewed on 2025-11-14 13:53_

---

_Review comment by @zanieb on `crates/uv-client/src/linehaul.rs`:93 on 2025-11-14 13:53_

This shouldn't be included in the code

---

_@zanieb reviewed on 2025-11-14 13:53_

---

_Review comment by @zanieb on `crates/uv-client/src/linehaul.rs`:100 on 2025-11-14 13:53_

Why are we unwrapping here if `Distro` takes `Option`s?

---

_Review comment by @zanieb on `crates/uv-client/src/linehaul.rs`:104 on 2025-11-14 13:54_

Does the crate not provide an API for this? Should we ask for it?

---

_@zanieb reviewed on 2025-11-14 13:54_

---

_@zanieb reviewed on 2025-11-14 13:54_

---

_Review comment by @zanieb on `crates/uv-client/tests/it/user_agent_version.rs`:275 on 2025-11-14 13:54_

If we need to do transformations like this, we should not repeat it

---

_@zanieb reviewed on 2025-11-14 13:55_

---

_Review comment by @zanieb on `crates/uv-performance-memory-allocator/src/lib.rs`:12 on 2025-11-14 13:55_

This change to jemalloc should be a different pull request.

---

_@zanieb reviewed on 2025-11-14 13:56_

---

_Review comment by @zanieb on `crates/uv-python/src/windows_registry.rs`:32 on 2025-11-14 13:56_

Why is this Windows code changing?

---

_Renamed from "Replace `sys-info` with [sysinfo](https://crates.io/crates/sysinfo) library for better compatibility and Android support, and exclude Android from jemalloc to fix Termux compilation issues（is trying）." to "Replace `sys-info` with `sysinfo` for Android support; use mimalloc instead of jemalloc on Android" by @zanieb on 2025-11-14 13:57_

---

_@LIghtJUNction reviewed on 2025-11-14 15:14_

---

_Review comment by @LIghtJUNction on `crates/uv-performance-memory-allocator/src/lib.rs`:12 on 2025-11-14 15:14_

I think so, too. I will reissue a new pr.

---

_@LIghtJUNction reviewed on 2025-11-14 15:18_

---

_Review comment by @LIghtJUNction on `crates/uv-client/src/linehaul.rs`:104 on 2025-11-14 15:18_

https://docs.rs/sysinfo/latest/sysinfo/
I'm worried about destroying the existing compatibility. If not, how should I write it?

---

_@LIghtJUNction reviewed on 2025-11-14 15:20_

---

_Review comment by @LIghtJUNction on `crates/uv-client/tests/it/user_agent_version.rs`:275 on 2025-11-14 15:20_

You are quite right. If I pass the test, I will change it.

---

_@LIghtJUNction reviewed on 2025-11-14 15:26_

---

_Review comment by @LIghtJUNction on `Cargo.toml`:83 on 2025-11-14 15:26_

Here I use a vscode plugin: Dependi, for dependency upgrades.

---

_@zanieb reviewed on 2025-11-14 15:44_

---

_Review comment by @zanieb on `Cargo.toml`:83 on 2025-11-14 15:44_

Regardless, we shouldn't change the versions a bunch of dependencies like this. You'll need to reduce it to the relevant changes for `sysinfo`

---

_@konstin reviewed on 2025-11-14 15:48_

---

_Review comment by @konstin on `Cargo.toml`:83 on 2025-11-14 15:48_

A PR should have a small change and avoid making unrelated changes. That's important in a project as large as uv this is important for reviews (here specifically, we audit each dependency update separately), to keep the discussion in one PR focussed on a single things, and also for identifying and rolling back regressions if they happen. If the PR is about a dependency change, it should only change that dependency and make the necessary code changes, but not change other dependencies too.

---

_Comment by @LIghtJUNction on 2025-11-14 16:12_

Hello uv developers,
 
Greetings to all of you! Based on the principle of single responsibility, this pull request (PR) is essentially a test for issue identification. Through this test, we have identified two key issues that require your attention:
 
First, the dependency library "sys-info" needs to be uniformly updated to "sysinfo" for consistency across the codebase.
 
Second, version upgrades may trigger compatibility errors—among these, the error types specific to the Windows platform deserve special consideration. This issue has already emerged during the compilation process and carries a risk of recurrence in subsequent usage scenarios. We recommend that you proactively account for this in your relevant development work to avoid potential disruptions.

---

_Closed by @LIghtJUNction on 2025-11-16 05:16_

---
