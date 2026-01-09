---
number: 11641
title: Rule A003 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2024-05-31T14:37:10Z
updated_at: 2024-05-31T19:04:37Z
url: https://github.com/astral-sh/ruff/issues/11641
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule A003 cause panic

---

_Issue opened by @qarmin on 2024-05-31 14:37_

ruff 0.4.6 (latest changes from main branch)
```
ruff  *.py --select A003 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```

 mp} assertot yt
s
__init__ impz
```

error
```
All checks passed!

error: Panicked while linting /home/rafal/test/tmp_folder/15358500963919727445.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_python_codegen/src/stylist.rs:112:51:
byte index 1 is not a char boundary; it is inside '\u{a0}' (bytes 0..2) of ` mp} assertot yt`
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
   1: std::panicking::rust_panic_with_hook
   2: std::panicking::begin_panic_handler::{{closure}}
   3: std::sys_common::backtrace::__rust_end_short_backtrace
   4: rust_begin_unwind
   5: core::panicking::panic_fmt
   6: core::str::slice_error_fail_rt
   7: core::str::slice_error_fail
   8: ruff_python_codegen::stylist::Stylist::from_tokens
   9: ruff_linter::linter::lint_fix
  10: ruff::diagnostics::lint_path
  11: ruff::commands::check::lint_path
  12: rayon::iter::plumbing::Producer::fold_with
  13: rayon::iter::plumbing::bridge_producer_consumer::helper
  14: ruff::commands::check::check
  15: ruff::check
  16: ruff::run
  17: ruff::main
  18: std::sys_common::backtrace::__rust_begin_short_backtrace
  19: std::rt::lang_start::{{closure}}
  20: std::rt::lang_start_internal
  21: main
  22: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  23: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  24: _start

```

[python_compressed.zip](https://github.com/user-attachments/files/15515226/python_compressed.zip)


---

_Label `bug` added by @MichaReiser on 2024-05-31 14:51_

---

_Comment by @MichaReiser on 2024-05-31 14:51_

Hmm, I don't see anything obvious here that's triggering this but certainly worth looking into. @charliermarsh you merged a PR for this yesterday. 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-31 14:59_

---

_Comment by @charliermarsh on 2024-05-31 14:59_

I can take it.

---

_Label `fuzzer` added by @charliermarsh on 2024-05-31 14:59_

---

_Referenced in [astral-sh/ruff#11645](../../astral-sh/ruff/pulls/11645.md) on 2024-05-31 19:01_

---

_Closed by @charliermarsh on 2024-05-31 19:04_

---
