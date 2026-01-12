```yaml
number: 12165
title: Checking file with rule W605 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2024-07-03T10:04:20Z
updated_at: 2024-07-04T05:10:27Z
url: https://github.com/astral-sh/ruff/issues/12165
synced_at: 2026-01-12T15:54:51Z
```

# Checking file with rule W605 cause panic

---

_@qarmin_

ruff 0.5.0+130 (7c8112614 2024-07-02)
```
ruff *.py --select W605 --no-cache  --preview --output-format concise --isolated
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

error: Panicked while linting /home/rafal/test/tmp_folder/13825409523151480423.py: This indicates a bug in Ruff. If you could open an issue at:

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
   9: core::str::traits::<impl core::slice::index::SliceIndex<str> for core::ops::range::Range<usize>>::index
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/core/src/str/traits.rs:244:21
  10: core::str::traits::<impl core::ops::index::Index<I> for str>::index
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/core/src/str/traits.rs:62:15
  11: ruff_text_size::range::<impl core::ops::index::Index<ruff_text_size::range::TextRange> for str>::index
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_text_size/src/range.rs:430:14
  12: ruff_source_file::locator::Locator::slice
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_source_file/src/locator.rs:463:23
  13: ruff_linter::rules::pycodestyle::rules::invalid_escape_sequence::check
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs:133:18
  14: ruff_linter::rules::pycodestyle::rules::invalid_escape_sequence::invalid_escape_sequence
  15: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
  16: ruff_python_ast::visitor::walk_expr
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_python_ast/src/visitor.rs:543:17
  17: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
  18: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
  19: ruff_linter::checkers::ast::Checker::visit_deferred_string_type_definitions
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2203:21
  20: ruff_linter::checkers::ast::Checker::visit_deferred
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2286:13
  21: ruff_linter::checkers::ast::check_ast
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2414:5
  22: ruff_linter::linter::check_path
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/linter.rs:146:36
  23: ruff_linter::linter::lint_only
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/linter.rs:409:23
  24: ruff::diagnostics::lint_path
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/diagnostics.rs:321:22
  25: ruff::commands::check::lint_path::{{closure}}
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/commands/check.rs:192:9
  26: std::panicking::try::do_call
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:559:40
  27: std::panicking::try
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:523:19
  28: std::panic::catch_unwind
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panic.rs:149:14
  29: ruff::panic::catch_unwind
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/panic.rs:40:18
  30: ruff::commands::check::lint_path
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/commands/check.rs:191:18
  31: ruff::commands::check::check::{{closure}}
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/commands/check.rs:93:17
  32: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  33: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  34: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  35: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  36: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  37: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  38: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  39: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  40: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  41: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  42: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  43: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  44: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  45: ruff::commands::check::check
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/commands/check.rs:163:10
  46: ruff::check
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/lib.rs:413:13
  47: ruff::run
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/lib.rs:186:33
  48: ruff::main
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff/src/main.rs:85:11
  49: core::ops::function::FnOnce::call_once
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/core/src/ops/function.rs:250:5
  50: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/sys_common/backtrace.rs:155:18
  51: std::rt::lang_start::{{closure}}
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/rt.rs:159:18
  52: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/core/src/ops/function.rs:284:13
  53: std::panicking::try::do_call
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:559:40
  54: std::panicking::try
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:523:19
  55: std::panic::catch_unwind
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panic.rs:149:14
  56: std::rt::lang_start_internal::{{closure}}
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/rt.rs:141:48
  57: std::panicking::try::do_call
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:559:40
  58: std::panicking::try
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:523:19
  59: std::panic::catch_unwind
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panic.rs:149:14
  60: std::rt::lang_start_internal
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/rt.rs:141:20
  61: main
  62: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  63: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  64: _start

```

Ruff build, that was used to reproduce problem(compiled on Ubuntu 22.04 with relase mode + debug symbols + debug assertions + overflow checks) - https://github.com/qarmin/Automated-Fuzzer/releases/download/Nightly/ruff.7z

[python_compressed.zip](https://github.com/user-attachments/files/16082041/python_compressed.zip)


---

_Comment by @MichaReiser on 2024-07-03 13:52_

Huh, the position where the rule is panicking would suggest that the range on a literal is incorrect. It might only be happening inside of the type annotation.

---

_Label `bug` added by @MichaReiser on 2024-07-03 13:52_

---

_Comment by @dhruvmanila on 2024-07-04 05:10_

Yeah, I can confirm that this is related to https://github.com/astral-sh/ruff/issues/10586. You can see that the range of the inner strings are relative to the outer string:
```
String(
    ExprStringLiteral {
        range: 122..158,
        value: StringLiteralValue {
            inner: Single(
                StringLiteral {
                    range: 11..15,
                    value: "\n",
                    flags: StringLiteralFlags {
                        quote_style: Single,
                        prefix: Empty,
                        triple_quoted: false,
                    },
                },
            ),
        },
    },
)
String(
    ExprStringLiteral {
        range: 122..158,
        value: StringLiteralValue {
            inner: Single(
                StringLiteral {
                    range: 17..23,
                    value: "\r\n",
                    flags: StringLiteralFlags {
                        quote_style: Single,
                        prefix: Empty,
                        triple_quoted: false,
                    },
                },
            ),
        },
    },
)
String(
    ExprStringLiteral {
        range: 122..158,
        value: StringLiteralValue {
            inner: Single(
                StringLiteral {
                    range: 25..29,
                    value: "\r",
                    flags: StringLiteralFlags {
                        quote_style: Single,
                        prefix: Empty,
                        triple_quoted: false,
                    },
                },
            ),
        },
    },
)
```
I'll close this in favor of #10586 

---

_Closed by @dhruvmanila on 2024-07-04 05:10_

---
