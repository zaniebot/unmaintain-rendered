---
number: 9655
title: Rule RET505(and also 506,507,508) cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2024-01-27T06:20:59Z
updated_at: 2024-01-27T14:15:34Z
url: https://github.com/astral-sh/ruff/issues/9655
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule RET505(and also 506,507,508) cause panic

---

_Issue opened by @qarmin on 2024-01-27 06:20_

Ruff 0.1.14 (latest changes from main branch)
```
ruff  *.py --select RET505 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
def video_extern_exists_intern(video_id, db_interventions) -> bool:
    if _: return True
    else: return False
```

error
```

error: Panicked while linting /home/rafal/test/tmp_folder/F_NAME_426977598637422446.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/rafal/test/ruff/crates/ruff_text_size/src/range.rs:48:9:
assertion failed: start.raw <= end.raw
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/alloc/src/boxed.rs:2021:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:783:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:649:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/sys_common/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:645:5
   6: core::panicking::panic_fmt
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/panicking.rs:72:14
   7: core::panicking::panic
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/panicking.rs:127:5
   8: ruff_linter::rules::flake8_return::rules::function::remove_else
   9: ruff_linter::rules::flake8_return::rules::function::function
  10: ruff_linter::checkers::ast::analyze::statement::statement
  11: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
  12: ruff_linter::checkers::ast::check_ast
  13: ruff_linter::linter::check_path
  14: ruff_linter::linter::lint_fix
  15: ruff::diagnostics::lint_path
  16: ruff::panic::catch_unwind
  17: ruff::commands::check::lint_path
  18: rayon::iter::plumbing::bridge_producer_consumer::helper
  19: ruff::commands::check::check
  20: ruff::check
  21: ruff::run
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

[python_compressed.zip](https://github.com/astral-sh/ruff/files/14071334/python_compressed.zip)


---

_Comment by @charliermarsh on 2024-01-27 14:01_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-27 14:01_

---

_Label `bug` added by @charliermarsh on 2024-01-27 14:01_

---

_Referenced in [astral-sh/ruff#9657](../../astral-sh/ruff/pulls/9657.md) on 2024-01-27 14:10_

---

_Closed by @charliermarsh on 2024-01-27 14:15_

---
