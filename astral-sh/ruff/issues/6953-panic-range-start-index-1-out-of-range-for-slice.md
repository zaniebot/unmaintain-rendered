```yaml
number: 6953
title: "Panic \"'range start index 1 out of range for slice of length 0'\""
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-08-28T20:00:35Z
updated_at: 2023-08-28T21:03:20Z
url: https://github.com/astral-sh/ruff/issues/6953
synced_at: 2026-01-12T15:54:46Z
```

# Panic "'range start index 1 out of range for slice of length 0'"

---

_@qarmin_

Ruff 0.0.286 (latest changes from main branch)

Code caomes from real world package but it is mininized

```
ruff  *.py --select ALL --no-cache
```

file content:
```
from typing import NamedTuple
try:
    import collections.abc as collections_abc
except ImportError:
    from test import mod_generics_cache
class BaseTestCase(TestCase):
           Emp = NamedTuple(typename='Emp', name=str, id=int)
```

error:
```
panicked at 'range start index 1 out of range for slice of length 0', crates/ruff/src/rules/pyupgrade/rules/convert_named_tuple_functional_to_class.rs:212:30
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
   7: core::slice::index::slice_start_index_len_fail_rt
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/slice/index.rs:52:5
   8: core::slice::index::slice_start_index_len_fail
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/slice/index.rs:40:9
   9: ruff::checkers::ast::analyze::statement::statement
  10: <ruff::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
  11: <ruff::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
  12: ruff::checkers::ast::check_ast
  13: ruff::linter::check_path
  14: ruff::linter::lint_fix
  15: ruff_cli::diagnostics::lint_path
  16: rayon::iter::plumbing::bridge_producer_consumer::helper
  17: ruff_cli::commands::run::run
  18: ruff_cli::check
  19: ruff_cli::run
  20: ruff::main
  21: std::sys_common::backtrace::__rust_begin_short_backtrace
  22: main
  23: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  24: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  25: _start

```



[test_typing-1040340015335742303559550111542157.py.zip](https://github.com/astral-sh/ruff/files/12457830/test_typing-1040340015335742303559550111542157.py.zip)


---

_Label `bug` added by @zanieb on 2023-08-28 20:05_

---

_Label `fuzzer` added by @zanieb on 2023-08-28 20:05_

---

_Comment by @charliermarsh on 2023-08-28 20:16_

We're not handling `typename='Emp'` correctly -- we're assuming it's provided as a positional argument.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-28 20:31_

---

_Closed by @charliermarsh on 2023-08-28 21:03_

---
