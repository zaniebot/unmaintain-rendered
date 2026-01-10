```yaml
number: 9323
title: Rule W293 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-12-30T21:10:22Z
updated_at: 2023-12-31T15:43:36Z
url: https://github.com/astral-sh/ruff/issues/9323
synced_at: 2026-01-10T11:09:51Z
```

# Rule W293 cause panic

---

_Issue opened by @qarmin on 2023-12-30 21:10_

Ruff 0.1.9 (latest changes from main branch)
```
ruff  *.py --select W293 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
class Chassis(RobotModuleTemplate):
        """底盘信息推送控制
        
        """\
```

error
```

error: Panicked while linting /home/rafal/test/tmp_folder/F_NAME_1069023761531669644715.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_source_file/src/locator.rs:75:34:
byte index 70 is not a char boundary; it is inside '制' (bytes 68..71) of `class Chassis(RobotModuleTemplate):
        """底盘信息推送控制
        
        """\
`
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
   9: ruff_source_file::locator::Locator::line_start
  10: ruff_python_index::indexer::Indexer::preceded_by_continuations
  11: ruff_linter::rules::pycodestyle::rules::trailing_whitespace::trailing_whitespace
  12: ruff_linter::checkers::physical_lines::check_physical_lines
  13: ruff_linter::linter::check_path
  14: ruff_linter::linter::lint_fix
  15: ruff_cli::diagnostics::lint_path
  16: ruff_cli::panic::catch_unwind
  17: ruff_cli::commands::check::lint_path
  18: rayon::iter::plumbing::bridge_producer_consumer::helper
  19: ruff_cli::commands::check::check
  20: ruff_cli::check
  21: ruff_cli::run
  22: ruff::main
  23: std::sys_common::backtrace::__rust_begin_short_backtrace
  24: std::rt::lang_start::{{closure}}
  25: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/ops/function.rs:284:13
  26: std::panicking::try::do_call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:552:40
  27: std::panicking::try
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:516:19
  28: std::panic::catch_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panic.rs:142:14
  29: std::rt::lang_start_internal::{{closure}}
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/rt.rs:148:48
  30: std::panicking::try::do_call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:552:40
  31: std::panicking::try
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:516:19
  32: std::panic::catch_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panic.rs:142:14
  33: std::rt::lang_start_internal
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/rt.rs:148:20
  34: std::rt::lang_start
  35: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  36: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  37: _start


```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/13799181/python_compressed.zip)


---

_Label `bug` added by @charliermarsh on 2023-12-31 12:40_

---

_Label `fuzzer` added by @charliermarsh on 2023-12-31 12:40_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-31 12:50_

---

_Closed by @charliermarsh on 2023-12-31 15:43_

---
