---
number: 8430
title: "Windows Clippy job failing with `GLIBC_2.39` not found"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - internal
assignees: []
created_at: 2024-10-21T22:28:48Z
updated_at: 2025-02-13T20:36:29Z
url: https://github.com/astral-sh/uv/issues/8430
synced_at: 2026-01-10T01:24:28Z
---

# Windows Clippy job failing with `GLIBC_2.39` not found

---

_Issue opened by @zanieb on 2024-10-21 22:28_

```
Run cargo xwin clippy --target x86_64-pc-windows-msvc --workspace --all-targets --all-features --locked --profile fast-build -- -D warnings
    Updating crates.io index
    Checking windows-sys v0.48.0
   Compiling libz-ng-sys v1.1.[16](https://github.com/astral-sh/uv/actions/runs/11446764894/job/31846667675#step:8:17)
   Compiling schemars v0.8.21
error: failed to run custom build command for `libz-ng-sys v1.1.16`

Caused by:
  process didn't exit successfully: `/home/runner/work/uv/uv/target/fast-build/build/libz-ng-sys-2247228bd72b72ab/build-script-cmake` (exit status: 1)
  --- stderr
  /home/runner/work/uv/uv/target/fast-build/build/libz-ng-sys-[22](https://github.com/astral-sh/uv/actions/runs/11446764894/job/31846667675#step:8:23)47228bd72b72ab/build-script-cmake: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/work/uv/uv/target/fast-build/build/libz-ng-sys-2247228bd72b72ab/build-script-cmake)
```

---

_Label `help wanted` added by @zanieb on 2024-10-21 22:28_

---

_Label `internal` added by @zanieb on 2024-10-21 22:28_

---

_Comment by @zanieb on 2024-10-21 22:29_

Please someone save me from these horrendous GitHub Actions behaviors.

---

_Comment by @zanieb on 2024-10-22 12:18_

Doesn't look runner version related?

Failed run
```
Current runner version: '2.320.0'
Operating System
  Ubuntu
  22.04.5
  LTS
Runner Image
  Image: ubuntu-22.04
  Version: 20241015.1.0
  Included Software: https://github.com/actions/runner-images/blob/ubuntu22/20241015.1/images/ubuntu/Ubuntu2204-Readme.md
  Image Release: https://github.com/actions/runner-images/releases/tag/ubuntu22%2F20241015.1
```

Succeeding run
```
Current runner version: '2.320.0'
Operating System
  Ubuntu
  22.04.5
  LTS
Runner Image
  Image: ubuntu-22.04
  Version: 20241015.1.0
  Included Software: https://github.com/actions/runner-images/blob/ubuntu22/20241015.1/images/ubuntu/Ubuntu2204-Readme.md
  Image Release: https://github.com/actions/runner-images/releases/tag/ubuntu22%2F20241015.1
```

---

_Referenced in [astral-sh/uv#8466](../../astral-sh/uv/pulls/8466.md) on 2024-10-22 16:06_

---

_Closed by @zanieb on 2024-11-06 22:11_

---

_Comment by @SRv6d on 2025-02-13 20:36_

Having the same issues on runner version '2.322.0', what ended up being the fix?

---
