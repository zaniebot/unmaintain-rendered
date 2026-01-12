```yaml
number: 14396
title: "Fixing file with rules PYI041, RUF013, UP007 cause panic (`redundant_numeric_union`)"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2024-11-17T06:28:06Z
updated_at: 2024-11-17T17:15:46Z
url: https://github.com/astral-sh/ruff/issues/14396
synced_at: 2026-01-12T15:54:53Z
```

# Fixing file with rules PYI041, RUF013, UP007 cause panic (`redundant_numeric_union`)

---

_@qarmin_

ruff 0.7.4+1285 (4dcb7ddaf 2024-11-16)
```
ruff check *.py --select PYI041,RUF013,UP007 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
from __future__ import annotations
from typing import TYPE_CHECKING
if TYPE_CHECKING:
 from typing import Union
def s(r: Union[int,float]=None):""
```

error
```
All checks passed!

error: Panicked while linting /tmp/tmp_folder/data/2330183557080594465.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs:159:26:
called `Option::unwrap()` on a `None` value
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/alloc/src/boxed.rs:1982:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/std/src/panicking.rs:809:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/std/src/panicking.rs:667:13
   4: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/std/src/sys/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/std/src/panicking.rs:665:5
   6: core::panicking::panic_fmt
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/core/src/panicking.rs:76:14
   7: core::panicking::panic
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/core/src/panicking.rs:148:5
   8: core::option::unwrap_failed
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/core/src/option.rs:2009:5
   9: ruff_linter::rules::flake8_pyi::rules::redundant_numeric_union::check_annotation
  10: ruff_linter::rules::flake8_pyi::rules::redundant_numeric_union::redundant_numeric_union
             at ./ruff/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs:86:9
  11: ruff_linter::checkers::ast::analyze::statement::statement
             at ./ruff/crates/ruff_linter/src/checkers/ast/analyze/statement.rs:181:17
  12: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1046:9
  13: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_body
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1667:13
  14: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2533:5
  15: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:148:36
  16: ruff_linter::linter::lint_fix
             at ./ruff/crates/ruff_linter/src/linter.rs:513:27
  17: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:278:14
  18: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:195:9
  19: std::panicking::try::do_call
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:557:40
  20: __rust_try
  21: std::panicking::try
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:520:19
  22: std::panic::catch_unwind
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
  23: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  24: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:194:18
  25: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:96:17
  26: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  27: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  28: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  29: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  30: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  31: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  32: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  33: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  34: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  35: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  36: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  37: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  38: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  39: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:166:10
  40: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:416:13
  41: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:188:33
  42: ruff::main
             at ./ruff/crates/ruff/src/main.rs:44:11
  43: core::ops::function::FnOnce::call_once
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  44: std::sys::backtrace::__rust_begin_short_backtrace
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:154:18
  45: std::rt::lang_start::{{closure}}
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:195:18
  46: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/core/src/ops/function.rs:284:13
  47: std::panicking::try::do_call
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/std/src/panicking.rs:557:40
  48: std::panicking::try
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/std/src/panicking.rs:520:19
  49: std::panic::catch_unwind
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/std/src/panic.rs:358:14
  50: std::rt::lang_start_internal::{{closure}}
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/std/src/rt.rs:174:48
  51: std::panicking::try::do_call
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/std/src/panicking.rs:557:40
  52: std::panicking::try
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/std/src/panicking.rs:520:19
  53: std::panic::catch_unwind
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/std/src/panic.rs:358:14
  54: std::rt::lang_start_internal
             at /rustc/917a50a03931a9861c19a46f3e2a02a28f1da936/library/std/src/rt.rs:174:20
  55: std::rt::lang_start
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:194:17
  56: <unknown>
  57: __libc_start_main
  58: _start

```

Ruff build, that was used to reproduce problem(compiled on Ubuntu 22.04 with relase mode + debug symbols + debug assertions + overflow checks) - https://github.com/qarmin/Automated-Fuzzer/releases/download/Nightly/ruff.7z

[python_compressed.zip](https://github.com/user-attachments/files/17789101/python_compressed.zip)


---

_Label `bug` added by @MichaReiser on 2024-11-17 09:14_

---

_Comment by @MichaReiser on 2024-11-17 09:14_

CC: @sbrugman 

---

_Label `fuzzer` added by @AlexWaygood on 2024-11-17 11:44_

---

_Comment by @charliermarsh on 2024-11-17 16:42_

Oh sorry, I merged this without addressing so I'll fix it quickly.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-17 16:42_

---

_Closed by @charliermarsh on 2024-11-17 17:15_

---

_Closed by @charliermarsh on 2024-11-17 17:15_

---
