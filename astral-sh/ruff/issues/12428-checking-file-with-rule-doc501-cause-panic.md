```yaml
number: 12428
title: Checking file with rule DOC501 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2024-07-21T12:48:13Z
updated_at: 2024-07-21T19:30:07Z
url: https://github.com/astral-sh/ruff/issues/12428
synced_at: 2026-01-10T11:09:54Z
```

# Checking file with rule DOC501 cause panic

---

_Issue opened by @qarmin on 2024-07-21 12:48_

ruff 0.5.4+277 (4bc73dd87 2024-07-20)
```
ruff *.py --select DOC501 --no-cache  --preview --output-format concise --isolated
```

file content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
def parse_bool(x, default=_parse_bool_sentinel):
    """Parse a boolean value
    bool or type(default)
    Raises
    `ValueError`
   ê>>> all(parse_bool(x) for x in [True, "yes", "Yes", "true", "True", "on", "ON", "1", 1])
    """
```

error
```
All checks passed!

error: Panicked while linting /tmp/tmp_folder/data/13947323996186240764.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs:213:38:
byte index 4 is not a char boundary; it is inside 'ê' (bytes 3..5) of `   ê>>> all(parse_bool(x) for x in [True, "yes", "Yes", "true", "True", "on", "ON", "1", 1])`
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
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
   9: core::str::traits::<impl core::slice::index::SliceIndex<str> for core::ops::range::RangeFrom<usize>>::index
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/core/src/str/traits.rs:441:21
  10: core::str::traits::<impl core::ops::index::Index<I> for str>::index
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/core/src/str/traits.rs:62:15
  11: ruff_linter::rules::pydoclint::rules::check_docstring::parse_entries_numpy
             at ./ruff/crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs:213:38
  12: ruff_linter::rules::pydoclint::rules::check_docstring::parse_entries
             at ./ruff/crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs:170:32
  13: ruff_linter::rules::pydoclint::rules::check_docstring::DocstringEntries::from_sections
             at ./ruff/crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs:151:40
  14: ruff_linter::rules::pydoclint::rules::check_docstring::check_docstring
  15: ruff_linter::checkers::ast::analyze::definitions::definitions
             at ./ruff/crates/ruff_linter/src/checkers/ast/analyze/definitions.rs:324:21
  16: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2430:5
  17: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:146:36
  18: ruff_linter::linter::lint_only
             at ./ruff/crates/ruff_linter/src/linter.rs:409:23
  19: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:321:22
  20: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:192:9
  21: std::panicking::try::do_call
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:559:40
  22: std::panicking::try
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:523:19
  23: std::panic::catch_unwind
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panic.rs:149:14
  24: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  25: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:191:18
  26: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:93:17
  27: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  28: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  29: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  30: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  31: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  32: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  33: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  34: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  35: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  36: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  37: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  38: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  39: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  40: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:163:10
  41: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:411:13
  42: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:186:33
  43: ruff::main
             at ./ruff/crates/ruff/src/main.rs:85:11
  44: core::ops::function::FnOnce::call_once
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/core/src/ops/function.rs:250:5
  45: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/sys_common/backtrace.rs:155:18
  46: std::rt::lang_start::{{closure}}
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/rt.rs:159:18
  47: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/core/src/ops/function.rs:284:13
  48: std::panicking::try::do_call
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:559:40
  49: std::panicking::try
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:523:19
  50: std::panic::catch_unwind
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panic.rs:149:14
  51: std::rt::lang_start_internal::{{closure}}
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/rt.rs:141:48
  52: std::panicking::try::do_call
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:559:40
  53: std::panicking::try
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panicking.rs:523:19
  54: std::panic::catch_unwind
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/panic.rs:149:14
  55: std::rt::lang_start_internal
             at /rustc/129f3b9964af4d4a709d1383930ade12dfe7c081/library/std/src/rt.rs:141:20
  56: main
  57: <unknown>
  58: __libc_start_main
  59: _start

```

[python_compressed.zip](https://github.com/user-attachments/files/16324485/python_compressed.zip)


---

_Label `bug` added by @charliermarsh on 2024-07-21 12:59_

---

_Label `fuzzer` added by @charliermarsh on 2024-07-21 12:59_

---

_Comment by @charliermarsh on 2024-07-21 12:59_

We should fix this before the next release.

---

_Comment by @augustelalande on 2024-07-21 17:36_

I'll check this

---

_Closed by @charliermarsh on 2024-07-21 19:30_

---

_Closed by @charliermarsh on 2024-07-21 19:30_

---
