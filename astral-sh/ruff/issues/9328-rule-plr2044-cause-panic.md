---
number: 9328
title: Rule PLR2044 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-12-31T08:26:37Z
updated_at: 2023-12-31T12:54:33Z
url: https://github.com/astral-sh/ruff/issues/9328
synced_at: 2026-01-10T01:22:49Z
---

# Rule PLR2044 cause panic

---

_Issue opened by @qarmin on 2023-12-31 08:26_

Ruff 0.1.9 (latest changes from main branch)
```
ruff  *.py --select PLR2044 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
ﻢ#
```

error
```

error: Panicked while linting /home/rafal/test/tmp_folder/F_NAME_5212019236686687639.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/rafal/test/ruff/crates/ruff_source_file/src/locator.rs:455:23:
byte index 1 is not a char boundary; it is inside 'ﻢ' (bytes 0..3) of `ﻢ#`
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/alloc/src/boxed.rs:2021:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:783:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:657:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/sys_common/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:645:5
   6: core::panicking::panic_fmt
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/panicking.rs:72:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/str/mod.rs:88:9
   9: ruff_linter::fix::fix_file
  10: ruff_linter::linter::lint_fix
  11: ruff_cli::diagnostics::lint_path
  12: ruff_cli::panic::catch_unwind
  13: ruff_cli::commands::check::lint_path
  14: rayon::iter::plumbing::bridge_producer_consumer::helper
  15: ruff_cli::commands::check::check
  16: ruff_cli::check
  17: ruff_cli::run
  18: ruff::main
  19: std::sys_common::backtrace::__rust_begin_short_backtrace
  20: std::rt::lang_start::{{closure}}
  21: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/ops/function.rs:284:13
  22: std::panicking::try::do_call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:552:40
  23: std::panicking::try
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:516:19
  24: std::panic::catch_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panic.rs:142:14
  25: std::rt::lang_start_internal::{{closure}}
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/rt.rs:148:48
  26: std::panicking::try::do_call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:552:40
  27: std::panicking::try
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:516:19
  28: std::panic::catch_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panic.rs:142:14
  29: std::rt::lang_start_internal
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/rt.rs:148:20
  30: std::rt::lang_start
  31: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  32: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  33: _start


```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/13800618/python_compressed.zip)



---

_Label `bug` added by @charliermarsh on 2023-12-31 12:41_

---

_Label `fuzzer` added by @charliermarsh on 2023-12-31 12:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-31 12:44_

---

_Referenced in [astral-sh/ruff#9331](../../astral-sh/ruff/pulls/9331.md) on 2023-12-31 12:49_

---

_Closed by @charliermarsh on 2023-12-31 12:54_

---
