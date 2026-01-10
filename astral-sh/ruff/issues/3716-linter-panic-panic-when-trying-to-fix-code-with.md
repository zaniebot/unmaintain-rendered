---
number: 3716
title: "[Linter panic] Panic when trying to fix code with arabic letters"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-24T18:33:46Z
updated_at: 2023-04-06T17:54:54Z
url: https://github.com/astral-sh/ruff/issues/3716
synced_at: 2026-01-10T01:22:42Z
---

# [Linter panic] Panic when trying to fix code with arabic letters

---

_Issue opened by @qarmin on 2023-03-24 18:33_

Ruff  1bac206995043d7d533338b7f8b7db37f37bf43b

```
ruff . --fix
```

File content 
```
sentences = [
    "ಆಪಲ್ ಒಂದು ಯು.ಕೆ. ಸ್ಟಾರ್ಟ್ಅಪ್ ಅನ್ನು ೧ ಶತಕೋಟಿ ಡಾಲರ್ಗಳಿಗೆ ಖರೀದಿಸಲು ನೋಡುತ್ತಿದೆ.",
    "ಸ್ವಾಯತ್ತ ಕಾರುಗಳು ವಿಾ ಹೊಣೆಗಾರಿಕೆಯನ್ನು ತಯಾರರ ಕಡೆಗೆ ಬದಲಾಯಿಸುತ್ತವೆ.",
    "ಕಾಲುದಾರಿ ವಿತರಣಾ ರೋಬೋಟ್‌ಗಳನ್ನು ನಿಷೇಧಿುವುದನ್ನು ಸ್ಯಾನ್ ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​ಪರಿಗಣಿಸುತ್ತದೆ.",
    "ಲಂಡನ್ ಯುನೈಟೆಡ್ ಕಿಂಗ್‌ಡಂನ ದೊಡ್ಡ ನಗರ.",    "ನೀನು ಎಲ್ಲಿದಿಯಾ?",
    "ಫ್ರಾನ್ಸಾದ ಅಧ್ಯಕ್ಷರು ಯಾರು?",
    "ಯುನೈಟೆಡ್ ಸ್ಟೇಟ್ಸ್ನ ರಾಜಧಾನಿ ಯಾವುದು?",
]
```
crash
```
panicked at 'begin <= end (876 <= 651) when slicing `sentences = [
    "ಆಪಲ್ ಒಂದು ಯು.ಕೆ. ಸ್ಟಾರ್ಟ್ಅಪ್ ಅನ್ನು ೧ ಶತಕೋಟಿ ಡಾಲರ್ಗಳಿಗೆ ಖರೀದಿಸಲು ನೋಡುತ್ತಿದೆ.",
    "ಸ್ವಾಯತ್ತ ಕ`[...]', /home/rafal/test/ruff/crates/ruff_python_ast/src/source_code/locator.rs:45:10
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/alloc/src/boxed.rs:2032:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:692:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:579:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/sys_common/backtrace.rs:137:18
   5: rust_begin_unwind
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:575:5
   6: core::panicking::panic_fmt
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/panicking.rs:64:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/str/mod.rs:86:9
   9: ruff_python_ast::source_code::locator::Locator::slice
  10: ruff::linter::lint_fix
  11: ruff_cli::diagnostics::lint_path
  12: rayon::iter::plumbing::bridge_producer_consumer::helper
  13: ruff_cli::commands::run::run
  14: ruff_cli::check
  15: ruff_cli::run
  16: ruff::main
  17: std::sys_common::backtrace::__rust_begin_short_backtrace
  18: main
  19: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  20: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:381:3
  21: _start

```

---

_Label `bug` added by @charliermarsh on 2023-03-24 20:59_

---

_Referenced in [astral-sh/ruff#3898](../../astral-sh/ruff/pulls/3898.md) on 2023-04-06 10:59_

---

_Closed by @charliermarsh on 2023-04-06 17:54_

---
