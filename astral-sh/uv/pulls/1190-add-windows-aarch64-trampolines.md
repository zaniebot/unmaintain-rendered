```yaml
number: 1190
title: Add windows aarch64 trampolines
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - windows
assignees: []
merged: true
base: main
head: konsti/windows-aarch64
created_at: 2024-01-30T17:26:36Z
updated_at: 2024-01-30T17:51:28Z
url: https://github.com/astral-sh/uv/pull/1190
synced_at: 2026-01-10T15:39:03Z
```

# Add windows aarch64 trampolines

---

_Pull request opened by @konstin on 2024-01-30 17:26_

Lacking windows compatible aarch64 hardware, i cross compiled the trampoline from x86_64 linux to aarch64-pc-windows-msvc; I added the instructions to the puffin-trampoline readme. With some testing on an aarch64 windows machine, this should be sufficient to build working win_arm64 tagged wheels.

i686-pc-windows-msvc is failing with an error:

```
error: linking with `lld-link` failed: exit status: 1
  = note: lld-link: error: undefined symbol: __aulldiv
          >>> referenced by libcompiler_builtins-2fb09dee087e9f64.rlib(compiler_builtins-2fb09dee087e9f64.compiler_builtins.597f0152646f1b8-cgu.0.rcgu.o):(compiler_builtins::int::specialized_div_rem::u128_div_rem::h06aed1e23a3f8f5c)
          >>> referenced by libcompiler_builtins-2fb09dee087e9f64.rlib(compiler_builtins-2fb09dee087e9f64.compiler_builtins.597f0152646f1b8-cgu.0.rcgu.o):(compiler_builtins::int::specialized_div_rem::u128_div_rem::h06aed1e23a3f8f5c)
          >>> referenced by libcompiler_builtins-2fb09dee087e9f64.rlib(compiler_builtins-2fb09dee087e9f64.compiler_builtins.597f0152646f1b8-cgu.0.rcgu.o):(compiler_builtins::int::specialized_div_rem::u128_div_rem::h06aed1e23a3f8f5c)
          >>> referenced 4 more times

          lld-link: error: undefined symbol: __aullrem
          >>> referenced by libcompiler_builtins-2fb09dee087e9f64.rlib(compiler_builtins-2fb09dee087e9f64.compiler_builtins.597f0152646f1b8-cgu.0.rcgu.o):(compiler_builtins::int::specialized_div_rem::u128_div_rem::h06aed1e23a3f8f5c)
          >>> referenced by libcompiler_builtins-2fb09dee087e9f64.rlib(compiler_builtins-2fb09dee087e9f64.compiler_builtins.597f0152646f1b8-cgu.0.rcgu.o):(compiler_builtins::int::specialized_div_rem::u128_div_rem::h06aed1e23a3f8f5c)
          >>> referenced by libcompiler_builtins-2fb09dee087e9f64.rlib(compiler_builtins-2fb09dee087e9f64.compiler_builtins.597f0152646f1b8-cgu.0.rcgu.o):(compiler_builtins::int::specialized_div_rem::u128_div_rem::h06aed1e23a3f8f5c)
          >>> referenced 4 more times
```

---

_Label `enhancement` added by @konstin on 2024-01-30 17:26_

---

_Label `windows` added by @konstin on 2024-01-30 17:26_

---

_@charliermarsh approved on 2024-01-30 17:27_

Oh cool! I still can't get it to compile on CI...

---

_Comment by @charliermarsh on 2024-01-30 17:27_

Due to `libgit2` failures: https://github.com/astral-sh/puffin/issues/1141

---

_Merged by @konstin on 2024-01-30 17:51_

---

_Closed by @konstin on 2024-01-30 17:51_

---

_Branch deleted on 2024-01-30 17:51_

---
