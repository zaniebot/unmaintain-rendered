```yaml
number: 8498
title: Fix hard and soft float libc detection for managed Python distributions on ARM
type: pull_request
state: merged
author: zarch
labels:
  - bug
assignees: []
merged: true
base: main
head: detect_hardfloat
created_at: 2024-10-23T11:34:58Z
updated_at: 2024-10-30T09:51:32Z
url: https://github.com/astral-sh/uv/pull/8498
synced_at: 2026-01-10T12:54:10Z
```

# Fix hard and soft float libc detection for managed Python distributions on ARM

---

_Pull request opened by @zarch on 2024-10-23 11:34_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This commit fix the errouneous detection of `vfp` from the cpu info.
The commit aims to fix issue #6873 integrating [PR7725](https://github.com/astral-sh/uv/pull/7725) developed by @kakkoyun 

## Test Plan

I've tested locally on my hardware, now it fails with:

```bash
❯ RUST_LOG=trace cargo run python install 3.12 --verbose
   Compiling uv-python v0.0.1 (/home/synapsees/source/uv/crates/uv-python)
   Compiling uv-types v0.0.1 (/home/synapsees/source/uv/crates/uv-types)
   Compiling uv-virtualenv v0.0.4 (/home/synapsees/source/uv/crates/uv-virtualenv)
   Compiling uv-distribution v0.0.1 (/home/synapsees/source/uv/crates/uv-distribution)
   Compiling uv-build-frontend v0.0.1 (/home/synapsees/source/uv/crates/uv-build-frontend)
   Compiling uv-resolver v0.0.1 (/home/synapsees/source/uv/crates/uv-resolver)
   Compiling uv-installer v0.0.1 (/home/synapsees/source/uv/crates/uv-installer)
   Compiling uv-settings v0.0.1 (/home/synapsees/source/uv/crates/uv-settings)
   Compiling uv-requirements v0.1.0 (/home/synapsees/source/uv/crates/uv-requirements)
   Compiling uv-dispatch v0.0.1 (/home/synapsees/source/uv/crates/uv-dispatch)
   Compiling uv-cli v0.0.1 (/home/synapsees/source/uv/crates/uv-cli)
   Compiling uv-tool v0.0.1 (/home/synapsees/source/uv/crates/uv-tool)
   Compiling uv-scripts v0.0.1 (/home/synapsees/source/uv/crates/uv-scripts)
   Compiling uv v0.4.16 (/home/synapsees/source/uv/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 2m 55s
     Running `target/debug/uv python install 3.12 --verbose`
DEBUG uv 0.4.16
TRACE Checking lock for `/home/synapsees/.local/share/uv/python` at `/home/synapsees/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/home/synapsees/.local/share/uv/python`
WARN Ignoring malformed managed Python entry:
    Failed to parse Python installation key `cpython-3.12.6-linux-armv7-gnueabihf`: invalid libc: Unknown libc environment: gnueabihf
WARN Ignoring malformed managed Python entry:
eabi
Searching for Python versions matching: Python 3.12
TRACE ld path: /lib/ld-linux-armhf.so.3
TRACE stdout output from `ld`: ""
DEBUG Using request timeout of 30s
DEBUG Released lock at `/home/synapsees/.local/share/uv/python/.lock`
error: Failed to parse Python installation key `cpython-3.12.6-linux-armv7-gnueabihf`: invalid libc: Unknown libc environment
```

But I believe this is due to #7975, already fixed by @zanieb therefore I would expect that once the changes are rebased everything should work.


---

_@kakkoyun approved on 2024-10-23 11:37_

LGTM. 

Thanks for picking it up.
I haven't had the chance find on-computer time.

---

_Review requested from @zanieb by @konstin on 2024-10-24 16:07_

---

_@zanieb approved on 2024-10-24 16:11_

---

_@zanieb approved on 2024-10-29 23:32_

Thanks!

---

_Renamed from "Fix hard float detection" to "Fix hard and soft float libc detection for managed Python distributions on ARM" by @zanieb on 2024-10-29 23:36_

---

_Label `bug` added by @zanieb on 2024-10-29 23:38_

---

_Merged by @zanieb on 2024-10-29 23:46_

---

_Closed by @zanieb on 2024-10-29 23:46_

---

_Branch deleted on 2024-10-30 09:51_

---
