```yaml
number: 11238
title: Rule ICN001 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2024-05-02T07:07:21Z
updated_at: 2024-05-02T17:59:40Z
url: https://github.com/astral-sh/ruff/issues/11238
synced_at: 2026-01-12T15:54:50Z
```

# Rule ICN001 cause panic

---

_@qarmin_

ruff 0.4.2 (latest changes from main branch)
```
ruff  *.py --select ICN001 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
import pandas
def init():
    pªndas.set_option('max_colwidth', None)
```

error
```
All checks passed!

error: Panicked while linting /opt/tmp_folder/6084416431827664882.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/renamer.rs:218:29:
assertion `left == right` failed
  left: 6
 right: 7
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/alloc/src/boxed.rs:2029:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:785:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:659:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/sys_common/backtrace.rs:171:18
   5: rust_begin_unwind
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:647:5
   6: core::panicking::panic_fmt
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/panicking.rs:72:14
   7: core::panicking::assert_failed_inner
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/panicking.rs:342:17
   8: core::panicking::assert_failed
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/panicking.rs:297:5
   9: ruff_linter::renamer::Renamer::rename_in_scope::{{closure}}
             at ./ruff/crates/ruff_linter/src/renamer.rs:218:29
  10: core::iter::adapters::map::map_fold::{{closure}}
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/iter/adapters/map.rs:89:28
  11: core::iter::adapters::copied::copy_fold::{{closure}}
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/iter/adapters/copied.rs:32:22
  12: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/slice/iter/macros.rs:232:27
  13: <core::iter::adapters::copied::Copied<I> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/iter/adapters/copied.rs:77:9
  14: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/iter/adapters/map.rs:129:9
  15: core::iter::traits::iterator::Iterator::for_each
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/iter/traits/iterator.rs:858:9
  16: alloc::vec::Vec<T,A>::extend_trusted
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/alloc/src/vec/mod.rs:2962:17
  17: <alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/alloc/src/vec/spec_extend.rs:26:9
  18: <alloc::vec::Vec<T,A> as core::iter::traits::collect::Extend<T>>::extend
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/alloc/src/vec/mod.rs:2904:9
  19: ruff_linter::renamer::Renamer::rename_in_scope
             at ./ruff/crates/ruff_linter/src/renamer.rs:210:23
  20: ruff_linter::renamer::Renamer::rename
             at ./ruff/crates/ruff_linter/src/renamer.rs:135:22
  21: ruff_linter::rules::flake8_import_conventions::rules::unconventional_import_alias::unconventional_import_alias::{{closure}}
             at ./ruff/crates/ruff_linter/src/rules/flake8_import_conventions/rules/unconventional_import_alias.rs:84:36
  22: ruff_diagnostics::diagnostic::Diagnostic::try_set_fix
             at ./ruff/crates/ruff_diagnostics/src/diagnostic.rs:57:15
  23: ruff_linter::rules::flake8_import_conventions::rules::unconventional_import_alias::unconventional_import_alias
             at ./ruff/crates/ruff_linter/src/rules/flake8_import_conventions/rules/unconventional_import_alias.rs:82:13
  24: ruff_linter::checkers::ast::analyze::bindings::bindings
             at ./ruff/crates/ruff_linter/src/checkers/ast/analyze/bindings.rs:59:39
  25: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2309:5
  26: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:156:40
  27: ruff_linter::linter::lint_fix
             at ./ruff/crates/ruff_linter/src/linter.rs:550:22
  28: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:280:14
  29: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:194:9
  30: std::panicking::try::do_call
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:554:40
  31: std::panicking::try
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:518:19
  32: std::panic::catch_unwind
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panic.rs:142:14
  33: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  34: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:193:18
  35: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:94:17
  36: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  37: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  38: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  39: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  40: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  41: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  42: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  43: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  44: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  45: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  46: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  47: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  48: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  49: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:165:10
  50: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:422:13
  51: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:199:33
  52: ruff::main
             at ./ruff/crates/ruff/src/main.rs:65:11
  53: core::ops::function::FnOnce::call_once
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/ops/function.rs:250:5
  54: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/sys_common/backtrace.rs:155:18
  55: std::rt::lang_start::{{closure}}
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/rt.rs:166:18
  56: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/ops/function.rs:284:13
  57: std::panicking::try::do_call
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:554:40
  58: std::panicking::try
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:518:19
  59: std::panic::catch_unwind
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panic.rs:142:14
  60: std::rt::lang_start_internal::{{closure}}
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/rt.rs:148:48
  61: std::panicking::try::do_call
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:554:40
  62: std::panicking::try
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:518:19
  63: std::panic::catch_unwind
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panic.rs:142:14
  64: std::rt::lang_start_internal
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/rt.rs:148:20
  65: main
  66: <unknown>
  67: __libc_start_main
  68: _start

```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/15184850/python_compressed.zip)



---

_Label `bug` added by @AlexWaygood on 2024-05-02 07:34_

---

_Label `fuzzer` added by @AlexWaygood on 2024-05-02 07:34_

---

_Closed by @charliermarsh on 2024-05-02 17:59_

---
