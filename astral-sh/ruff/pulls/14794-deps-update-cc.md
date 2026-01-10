```yaml
number: 14794
title: "deps: Update cc"
type: pull_request
state: merged
author: ognevny
labels:
  - internal
assignees: []
merged: true
base: main
head: bump-cc
created_at: 2024-12-05T17:02:50Z
updated_at: 2024-12-06T04:16:39Z
url: https://github.com/astral-sh/ruff/pull/14794
synced_at: 2026-01-10T20:42:27Z
```

# deps: Update cc

---

_Pull request opened by @ognevny on 2024-12-05 17:02_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

in unknown moment older versions became broken for windows-gnullvm targets. this update shouldn't break anything

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

successfully built for windows-gnullvm with `cargo build`

<!-- How was it tested? -->


---

_Comment by @zanieb on 2024-12-05 17:06_

Can you share more information about the breakage and how it's fixed upstream?

---

_@charliermarsh approved on 2024-12-05 17:07_

---

_Comment by @ognevny on 2024-12-05 17:09_

> Can you share more information about the breakage

<details><summary>from build log</summary>

```
  The following warnings were emitted during compilation:
  
  warning: libmimalloc-sys@0.1.39: clang: error: version 'llvm' in target triple 'x86_64-pc-windows-gnullvm' is invalid
  
  error: failed to run custom build command for `libmimalloc-sys v0.1.39`
  
  Caused by:
    process didn't exit successfully: `C:\_\B\src\ruff\target\release\build\libmimalloc-sys-d236219c533e2542\build-script-build` (exit code: 1)
    --- stdout
    OPT_LEVEL = Some("3")
    TARGET = Some("x86_64-pc-windows-gnullvm")
    HOST = Some("x86_64-pc-windows-gnullvm")
    cargo:rerun-if-env-changed=CC_x86_64-pc-windows-gnullvm
    CC_x86_64-pc-windows-gnullvm = None
    cargo:rerun-if-env-changed=CC_x86_64_pc_windows_gnullvm
    CC_x86_64_pc_windows_gnullvm = None
    cargo:rerun-if-env-changed=HOST_CC
    HOST_CC = None
    cargo:rerun-if-env-changed=CC
    CC = Some("clang")
    cargo:rerun-if-env-changed=CC_ENABLE_DEBUG_OUTPUT
    cargo:rerun-if-env-changed=CRATE_CC_NO_DEFAULTS
    CRATE_CC_NO_DEFAULTS = None
    DEBUG = Some("false")
    cargo:rerun-if-env-changed=CFLAGS_x86_64-pc-windows-gnullvm
    CFLAGS_x86_64-pc-windows-gnullvm = None
    cargo:rerun-if-env-changed=CFLAGS_x86_64_pc_windows_gnullvm
    CFLAGS_x86_64_pc_windows_gnullvm = None
    cargo:rerun-if-env-changed=HOST_CFLAGS
    HOST_CFLAGS = None
    cargo:rerun-if-env-changed=CFLAGS
    CFLAGS = Some("-march=nocona -msahf -mtune=generic -O2 -pipe -Wp,-D_FORTIFY_SOURCE=2 -fstack-protector-strong -Wp,-D__USE_MINGW_ANSI_STDIO=1")
    cargo:warning=clang: error: version 'llvm' in target triple 'x86_64-pc-windows-gnullvm' is invalid
  
    --- stderr
  
  
    error occurred: Command "clang" "-O3" "--target=x86_64-pc-windows-gnullvm" "-ffunction-sections" "-fdata-sections" "-m64" "--target=x86_64-pc-windows-gnullvm" "-march=nocona" "-msahf" "-mtune=generic" "-O2" "-pipe" "-Wp,-D_FORTIFY_SOURCE=2" "-fstack-protector-strong" "-Wp,-D__USE_MINGW_ANSI_STDIO=1" "-I" "c_src/mimalloc/include" "-I" "c_src/mimalloc/src" "-DMI_DEBUG=0" "-o" "C:\\_\\B\\src\\ruff\\target\\release\\build\\libmimalloc-sys-75e0ec94b36494b9\\out\\98cfcaec7182b1d8-static.o" "-c" "c_src/mimalloc/src/static.c" with args clang did not execute successfully (status code exit code: 1).
```

</details>

> how it's fixed upstream?

I have absolutely no idea! maybe something that happened with newer rustc, I honestly don't understand what happened, it worked with windows-gnullvm in the past



---

_Comment by @github-actions[bot] on 2024-12-05 17:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @charliermarsh on 2024-12-06 02:47_

---

_Closed by @charliermarsh on 2024-12-06 02:47_

---

_Label `internal` added by @charliermarsh on 2024-12-06 02:47_

---

_Branch deleted on 2024-12-06 04:16_

---
