```yaml
number: 9379
title: "Linter panic: f-string error location miscomputation"
type: issue
state: closed
author: addisoncrump
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2024-01-03T12:36:07Z
updated_at: 2024-01-04T05:00:56Z
url: https://github.com/astral-sh/ruff/issues/9379
synced_at: 2026-01-10T11:09:51Z
```

# Linter panic: f-string error location miscomputation

---

_Issue opened by @addisoncrump on 2024-01-03 12:36_

Similar to #5004, but occurs outside of the f-string.

Consider the following source snippet:

```py
F"{"ڤ
```

Which corresponds to the following hexdump:

```
00000000: 4622 7b22 daa4                           F"{"..
```

This results in the following crash dump due to an invalid char offset:

```
error: Panicked while linting test.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/addisoncrump/git/ruff/crates/ruff_source_file/src/locator.rs:396:23:
byte index 5 is not a char boundary; it is inside 'ڤ' (bytes 4..6) of `F"{"ڤ`
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/alloc/src/boxed.rs:2021:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:735:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:609:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/sys_common/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:597:5
   6: core::panicking::panic_fmt
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/panicking.rs:72:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/str/mod.rs:87:9
   9: ruff_linter::rules::pycodestyle::rules::errors::syntax_error
  10: ruff_linter::linter::check_path
  11: ruff_linter::linter::lint_only
  12: ruff_cli::diagnostics::lint_path
  13: ruff_cli::panic::catch_unwind
  14: ruff_cli::commands::check::lint_path
  15: rayon::iter::plumbing::Producer::fold_with
  16: rayon::iter::plumbing::bridge_producer_consumer::helper
  17: ruff_cli::commands::check::check
  18: ruff_cli::check
  19: ruff_cli::run
  20: ruff::main
  21: std::sys_common::backtrace::__rust_begin_short_backtrace
  22: std::rt::lang_start::{{closure}}
  23: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/ops/function.rs:284:13
  24: std::panicking::try::do_call
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:504:40
  25: std::panicking::try
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:468:19
  26: std::panic::catch_unwind
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panic.rs:142:14
  27: std::rt::lang_start_internal::{{closure}}
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/rt.rs:148:48
  28: std::panicking::try::do_call
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:504:40
  29: std::panicking::try
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:468:19
  30: std::panic::catch_unwind
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panic.rs:142:14
  31: std::rt::lang_start_internal
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/rt.rs:148:20
  32: main
  33: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  34: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:392:3
  35: _start
```

---

_Label `bug` added by @charliermarsh on 2024-01-04 04:06_

---

_Label `fuzzer` added by @charliermarsh on 2024-01-04 04:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-04 04:32_

---

_Closed by @charliermarsh on 2024-01-04 05:00_

---
