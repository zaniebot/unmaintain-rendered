```yaml
number: 8498
title: "not compile `flake8-to-ruff` (bin \"flake8-to-ruff\") due to previous error          ~/.../cryptography/ruff $ cargo run --bin flake8-to-ruff"
type: issue
state: closed
author: hotKe
labels: []
assignees: []
created_at: 2023-11-05T22:16:52Z
updated_at: 2023-11-06T04:34:19Z
url: https://github.com/astral-sh/ruff/issues/8498
synced_at: 2026-01-10T11:09:50Z
```

# not compile `flake8-to-ruff` (bin "flake8-to-ruff") due to previous error          ~/.../cryptography/ruff $ cargo run --bin flake8-to-ruff

---

_Issue opened by @hotKe on 2023-11-05 22:16_

,e6\x89\x8b\xe6\x95\xb2\xe7\x9a\x84\xe4\xbb\xa3\xe7\xa0\x81(\xe4\xb8\xad\xe6\x96\x87\xe6\xb3\xa8\xe9\x87\x8a)/chapter2/cryptography/ruff/target/debug/deps/flake8_to_ruff-e4a0abb456ac5239" "-Wl,--gc-sections" "-pie" "-Wl,-z,relro,-z,now" "-nodefaultlibs"                               = note: PLEASE submit a bug report to https://github.com/llvm/llvm-project/issues/ and include the crash backtrace.                                     cc: error: unable to execute command: Aborted                                                   cc: error: linker command failed due to signal (use -v to see invocation)                                                                                                             error: could not compile `flake8-to-ruff` (bin "flake8-to-ruff") due to previous

---

_Comment by @charliermarsh on 2023-11-06 00:28_

Sorry, can you share more information about what you're trying to do, and what error you ran into?

---

_Comment by @hotKe on 2023-11-06 01:21_

sr/lib/rustlib/aarch64-linux-android/lib/libcfg_if-25f2b0fabc3205fd.rlib" "/data/data/com.termux/files/usr/lib/rustlib/aarch64-linux-android/lib/liblibc-fdc5024e97210688.rlib" "/data/data/com.termux/files/usr/lib/rustlib/aarch64-linux-android/lib/liballoc-09acafac86c686dd.rlib" "/data/data/com.termux/files/usr/lib/rustlib/aarch64-linux-android/lib/librustc_std_workspace_core-399488a993d21146.rlib" "/data/data/com.termux/files/usr/lib/rustlib/aarch64-linux-android/lib/libcore-f1d02f19452ca532.rlib" "/data/data/com.termux/files/usr/lib/rustlib/aarch64-linux-android/lib/libcompiler_builtins-341cafe7ca1a2fe2.rlib" "-Wl,-Bdynamic" "-ldl" "-llog" "-lunwind" "-ldl" "-lm" "-lc" "-Wl,--eh-frame-hdr" "-Wl,-z,noexecstack" "-L" "/data/data/com.termux/files/usr/lib/rustlib/aarch64-linux-android/lib" "-o" "/data/data/com.termux/files/home/python-hacker-code/\xe6\x88\x91\xe6\x89\x8b\xe6\x95\xb2\xe7\x9a\x84\xe4\xbb\xa3\xe7\xa0\x81(\xe4\xb8\xad\xe6\x96\x87\xe6\xb3\xa8\xe9\x87\x8a)/chapter2/cryptography/ruff/target/debug/deps/flake8_to_ruff-e4a0abb456ac5239" "-Wl,--gc-sections" "-pie" "-Wl,-z,relro,-z,now" "-nodefaultlibs"
  = note: PLEASE submit a bug report to https://github.com/llvm/llvm-project/issues/ and include the crash backtrace.
          cc: error: unable to execute command: Aborted
          cc: error: linker command failed due to signal (use -v to see invocation)


warning: `flake8-to-ruff` (bin "flake8-to-ruff") generated 1 warning
error: could not compile `flake8-to-ruff` (bin "flake8-to-ruff") due to previous error; 1 warning emitted
~/.../cryptography/ruff $i use command in termux cargo run --bin flake8-to-ruff`

---

_Closed by @charliermarsh on 2023-11-06 04:34_

---
