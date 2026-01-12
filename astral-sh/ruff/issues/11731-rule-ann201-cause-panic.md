```yaml
number: 11731
title: Rule ANN201 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - parser
assignees: []
created_at: 2024-06-04T06:14:51Z
updated_at: 2024-06-04T08:45:48Z
url: https://github.com/astral-sh/ruff/issues/11731
synced_at: 2026-01-12T15:54:51Z
```

# Rule ANN201 cause panic

---

_@qarmin_

ruff 0.4.7 (latest changes from main branch)
```
ruff  *.py --select ANN201 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
﻿# Copyright (c) Microsoft Corporaton. All rights reserved.
if TYPE_CHECKING:
    def item(self, name: str) -> 'T':
                return x
```

error
```
All checks passed!

error: Panicked while linting /opt/tmp_folder/6194284649530868853.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_python_parser/src/lexer/cursor.rs:163:49:
byte index 114 is out of bounds of `# Copyright (c) Microsoft Corporaton. All rights reserved.
if TYPE_CHECKING:
    def item(self, name: str) -> 'T`
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/alloc/src/boxed.rs:2034:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:783:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:657:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/sys_common/backtrace.rs:171:18
   5: rust_begin_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:645:5
   6: core::panicking::panic_fmt
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/panicking.rs:72:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/str/mod.rs:92:9
   9: core::str::traits::<impl core::slice::index::SliceIndex<str> for core::ops::range::RangeTo<usize>>::index
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/str/traits.rs:367:21
  10: core::str::traits::<impl core::ops::index::Index<I> for str>::index
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/str/traits.rs:61:9
  11: ruff_python_parser::lexer::cursor::Cursor::skip_bytes
             at ./ruff/crates/ruff_python_parser/src/lexer/cursor.rs:163:49
  12: ruff_python_parser::lexer::Lexer::new
             at ./ruff/crates/ruff_python_parser/src/lexer.rs:108:13
  13: ruff_python_parser::token_source::TokenSource::from_source
             at ./ruff/crates/ruff_python_parser/src/token_source.rs:35:21
  14: ruff_python_parser::parser::Parser::new_starts_at
             at ./ruff/crates/ruff_python_parser/src/parser/mod.rs:60:22
  15: ruff_python_parser::parse_expression_range
             at ./ruff/crates/ruff_python_parser/src/lib.rs:163:5
  16: ruff_python_parser::typing::parse_simple_type_annotation
             at ./ruff/crates/ruff_python_parser/src/typing.rs:63:9
  17: ruff_python_parser::typing::parse_type_annotation
             at ./ruff/crates/ruff_python_parser/src/typing.rs:46:13
  18: ruff_linter::checkers::ast::Checker::visit_deferred_string_type_definitions
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2157:21
  19: ruff_linter::checkers::ast::Checker::visit_deferred
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2268:13
  20: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2396:5
  21: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:161:40
  22: ruff_linter::linter::lint_only
             at ./ruff/crates/ruff_linter/src/linter.rs:469:18
  23: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:318:22
  24: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:192:9
  25: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  26: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  27: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  28: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  29: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:191:18
  30: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:93:17
  31: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  32: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  33: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  34: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  35: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  36: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  37: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  38: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  39: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  40: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  41: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  42: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  43: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  44: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:163:10
  45: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:429:13
  46: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:202:33
  47: ruff::main
             at ./ruff/crates/ruff/src/main.rs:65:11
  48: core::ops::function::FnOnce::call_once
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/ops/function.rs:250:5
  49: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/sys_common/backtrace.rs:155:18
  50: std::rt::lang_start::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:166:18
  51: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/ops/function.rs:284:13
  52: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  53: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  54: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  55: std::rt::lang_start_internal::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:148:48
  56: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  57: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  58: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  59: std::rt::lang_start_internal
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:148:20
  60: main
  61: <unknown>
  62: __libc_start_main
  63: _start

```

[python_compressed.zip](https://github.com/user-attachments/files/15543364/python_compressed.zip)


---

_Comment by @MichaReiser on 2024-06-04 06:18_

@dhruvmanila any chance that's related to our parser changes?

---

_Label `bug` added by @MichaReiser on 2024-06-04 06:18_

---

_Label `parser` added by @MichaReiser on 2024-06-04 06:18_

---

_Comment by @dhruvmanila on 2024-06-04 06:20_

Let me check

---

_Comment by @dhruvmanila on 2024-06-04 06:23_

I'm unable to reproduce this on the latest `main`. @qarmin can you provide the raw file contents? It could be that GitHub is changing certain characters.

Edit: Nevermind, it's already in the PR description.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-06-04 06:29_

---

_Comment by @dhruvmanila on 2024-06-04 06:30_

Ok, I can reproduce it, it's related to the BOM character.

---

_Closed by @dhruvmanila on 2024-06-04 08:45_

---
