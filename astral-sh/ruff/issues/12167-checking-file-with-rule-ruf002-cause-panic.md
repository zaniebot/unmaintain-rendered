```yaml
number: 12167
title: Checking file with rule RUF002 cause panic
type: issue
state: closed
author: qarmin
labels: []
assignees: []
created_at: 2024-07-03T10:20:34Z
updated_at: 2024-07-04T05:26:36Z
url: https://github.com/astral-sh/ruff/issues/12167
synced_at: 2026-01-12T15:54:51Z
```

# Checking file with rule RUF002 cause panic

---

_@qarmin_

ruff 0.5.0+130 (7c8112614 2024-07-02)
```
ruff *.py --select RUF002 --no-cache  --preview --output-format concise --isolated
```

file content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
import typing as t
from Éutils import Namespace
if t.TYPE_CHECKING:
    import typing_extensions as te
NEWLINE_SEQUENCE: "te.Literal['\\n', '\\r\\n', '\\r']" = "\n"
```

error
```
All checks passed!

error: Panicked while linting /home/rafal/test/tmp_folder/4404989001469333526.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_source_file/src/locator.rs:463:23:
byte index 25 is not a char boundary; it is inside 'É' (bytes 24..26) of `import typing as t
from Éutils import Namespace
if t.TYPE_CHECKING:
    import typing_extensions as te
NEWLINE_SEQUENCE: "te.Literal['\\n', '\\r\\n', '\\r']" = "\n"`
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/alloc/src/boxed.rs:2036:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:799:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:664:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/sys_common/backtrace.rs:171:18
   5: rust_begin_unwind
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:652:5
   6: core::panicking::panic_fmt
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/core/src/panicking.rs:72:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/core/src/str/mod.rs:89:5
   9: ruff_linter::rules::ruff::rules::ambiguous_unicode_character::ambiguous_unicode_character_string
  10: ruff_linter::checkers::ast::analyze::string_like::string_like
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/checkers/ast/analyze/string_like.rs:13:9
  11: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
  12: ruff_python_ast::visitor::walk_expr
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_python_ast/src/visitor.rs:543:17
  13: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
  14: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
  15: ruff_linter::checkers::ast::Checker::visit_deferred_string_type_definitions
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2203:21
  16: ruff_linter::checkers::ast::Checker::visit_deferred
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2286:13
  17: ruff_linter::checkers::ast::check_ast
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2414:5
  18: ruff_linter::linter::check_path
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/linter.rs:146:36
  19: ruff_linter::linter::lint_only
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/linter.rs:409:23
  20: ruff::diagnostics::lint_path
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/diagnostics.rs:321:22
  21: ruff::commands::check::lint_path::{{closure}}
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/commands/check.rs:192:9
  22: std::panicking::try::do_call
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:559:40
  23: std::panicking::try
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:523:19
  24: std::panic::catch_unwind
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panic.rs:149:14
  25: ruff::panic::catch_unwind
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/panic.rs:40:18
  26: ruff::commands::check::lint_path
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/commands/check.rs:191:18
  27: ruff::commands::check::check::{{closure}}
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/commands/check.rs:93:17
  28: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  29: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  30: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  31: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  32: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  33: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  34: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  35: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  36: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  37: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  38: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  39: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  40: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  41: ruff::commands::check::check
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/commands/check.rs:163:10
  42: ruff::check
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/lib.rs:413:13
  43: ruff::run
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/lib.rs:186:33
  44: ruff::main
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/main.rs:85:11
  45: core::ops::function::FnOnce::call_once
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/core/src/ops/function.rs:250:5
  46: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/sys_common/backtrace.rs:155:18
  47: std::rt::lang_start::{{closure}}
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/rt.rs:159:18
  48: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/core/src/ops/function.rs:284:13
  49: std::panicking::try::do_call
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:559:40
  50: std::panicking::try
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:523:19
  51: std::panic::catch_unwind
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panic.rs:149:14
  52: std::rt::lang_start_internal::{{closure}}
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/rt.rs:141:48
  53: std::panicking::try::do_call
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:559:40
  54: std::panicking::try
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:523:19
  55: std::panic::catch_unwind
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panic.rs:149:14
  56: std::rt::lang_start_internal
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/rt.rs:141:20
  57: main
  58: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  59: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  60: _start

```

Ruff build, that was used to reproduce problem(compiled on Ubuntu 22.04 with relase mode + debug symbols + debug assertions + overflow checks) - https://github.com/qarmin/Automated-Fuzzer/releases/download/Nightly/ruff.7z

[python_compressed.zip](https://github.com/user-attachments/files/16082240/python_compressed.zip)


---

_Comment by @MichaReiser on 2024-07-03 13:54_

Same here, this is probably also related to https://github.com/astral-sh/ruff/issues/12165#issuecomment-2206138090

---

_Comment by @dhruvmanila on 2024-07-04 05:26_

I can confirm that this is similar to https://github.com/astral-sh/ruff/issues/12165#issuecomment-2208130739, closing in favor of #10586.

---

_Closed by @dhruvmanila on 2024-07-04 05:26_

---
