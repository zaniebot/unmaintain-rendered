```yaml
number: 6914
title: "Panicked at 'assertion failed: start.raw <= end.raw'"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-08-27T10:50:44Z
updated_at: 2023-08-27T15:27:09Z
url: https://github.com/astral-sh/ruff/issues/6914
synced_at: 2026-01-12T15:54:46Z
```

# Panicked at 'assertion failed: start.raw <= end.raw'

---

_@qarmin_

Ruff 0.0.286 (latest changes from main branch)

```
ruff  *.py --select ALL --no-cache
```

file content:
```
def pandas_is_installed():
    try:
        global pandas
        import pandas
    except ImportError:
        return False
```

error:
```
panicked at 'assertion failed: start.raw <= end.raw', /home/rafal/test/ruff/crates/ruff_text_size/src/range.rs:48:9
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/alloc/src/boxed.rs:2007:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:709:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:595:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/sys_common/backtrace.rs:151:18
   5: rust_begin_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:593:5
   6: core::panicking::panic_fmt
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panicking.rs:67:14
   7: core::panicking::panic
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panicking.rs:117:5
   8: ruff::linter::lint_fix
   9: ruff_cli::diagnostics::lint_path
  10: rayon::iter::plumbing::bridge_producer_consumer::helper
  11: ruff_cli::commands::run::run
  12: ruff_cli::check
  13: ruff_cli::run
  14: ruff::main
  15: std::sys_common::backtrace::__rust_begin_short_backtrace
  16: main
  17: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  18: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  19: _start

```


[test_functional-105134301910099611839452.py.zip](https://github.com/astral-sh/ruff/files/12447790/test_functional-105134301910099611839452.py.zip)


---

_Label `bug` added by @charliermarsh on 2023-08-27 13:44_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-27 13:44_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-27 14:56_

---

_Closed by @charliermarsh on 2023-08-27 15:27_

---
