---
number: 7012
title: "Panicked at 'byte index 1024 is not a char boundary; it is inside 'এ' (bytes 1022..1025) of `       \"অগ্রণী ব্যাংক\","
type: issue
state: closed
author: qarmin
labels:
  - bug
  - help wanted
  - fuzzer
assignees: []
created_at: 2023-08-30T21:46:54Z
updated_at: 2023-08-31T16:07:24Z
url: https://github.com/astral-sh/ruff/issues/7012
synced_at: 2026-01-07T13:12:15-06:00
---

# Panicked at 'byte index 1024 is not a char boundary; it is inside 'এ' (bytes 1022..1025) of `       "অগ্রণী ব্যাংক",

---

_Issue opened by @qarmin on 2023-08-30 21:46_

Ruff 0.0.286 (latest changes from main branch)

```
ruff  *.py --select ALL,NURSERY --no-cache
```

file content(I have more examples, and some may even be fully valid python files):
```
       "অগ্রণী ব্যাংক",
        "জনতা ব্যাংক",
        "রূপালী ব্যাংক",
        "সোনালী ব্যাংক",
        "বাংলাদেশ ডেভেলপমেন্ট ব্যাংক লিমিটেড",
        "বেসিক ব্যাংক লিমিটেড",
        "আইএফআইসি ব্যাংক লিমিটেড",
        "ইউনাইটেড কমার্শিয়াল ব্যাংক লিমিটেড",
        "ইস্টার্ন ব্যাংক লিমিটেড",
        "উত্তরা ব্যাংক",
        "এনআরবি কমার্শিয়াল ব্যাংক লিমিটেড",
        "এনআরবি গ্লোবাল ব্যাংক লিমিটেড",
        "এনআরবি ব্যাংক লিমিটেড",
        "এবি ব্যাংক লিমিটেড",
        "এ
```

error:
```
https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'byte index 1024 is not a char boundary; it is inside 'এ' (bytes 1022..1025) of `       "অগ্রণী ব্যাংক",
        "জনতা ব্যাংক",
        "রূপালী ব্যাংক",
        "সোনালী ব্যাংক",
        "বাংলাদেশ ডেভেলপমেন্ট`[...]', /home/rafal/test/ruff/crates/ruff_source_file/src/locator.rs:382:10
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/alloc/src/boxed.rs:2007:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:709:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:597:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/sys_common/backtrace.rs:151:18
   5: rust_begin_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:593:5
   6: core::panicking::panic_fmt
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panicking.rs:67:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/str/mod.rs:87:9
   9: ruff::linter::check_path
  10: ruff::linter::lint_fix
  11: ruff_cli::diagnostics::lint_path
  12: rayon::iter::plumbing::bridge_producer_consumer::helper
  13: ruff_cli::commands::check::check
  14: ruff_cli::check
  15: ruff_cli::run
  16: ruff::main
  17: std::sys_common::backtrace::__rust_begin_short_backtrace
  18: main
  19: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  20: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  21: _start
```


[PY_FILE_TEST_3316914300987.py.zip](https://github.com/astral-sh/ruff/files/12480364/PY_FILE_TEST_3316914300987.py.zip)


---

_Label `bug` added by @MichaReiser on 2023-08-31 06:11_

---

_Label `help wanted` added by @MichaReiser on 2023-08-31 06:18_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-31 13:31_

---

_Comment by @MichaReiser on 2023-08-31 16:07_

I created an issue describing the fix. Let's track the work in https://github.com/astral-sh/ruff/issues/7015

---

_Closed by @MichaReiser on 2023-08-31 16:07_

---

_Referenced in [astral-sh/ruff#7015](../../astral-sh/ruff/issues/7015.md) on 2023-08-31 16:12_

---
