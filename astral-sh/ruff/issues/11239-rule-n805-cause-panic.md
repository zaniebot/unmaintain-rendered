---
number: 11239
title: Rule N805 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2024-05-02T07:08:40Z
updated_at: 2024-05-02T17:59:41Z
url: https://github.com/astral-sh/ruff/issues/11239
synced_at: 2026-01-10T01:22:50Z
---

# Rule N805 cause panic

---

_Issue opened by @qarmin on 2024-05-02 07:08_

ruff 0.4.2 (latest changes from main branch)
```
ruff  *.py --select N805 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
class bc_climateÇaction_tax_credit_single_parent_household(Variable):
    def formula(household, period, parameters):
        married = hºusehold("is_married", period)
```

error
```
All checks passed!

error: Panicked while linting /opt/tmp_folder/6063404977126204582.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/renamer.rs:218:29:
assertion `left == right` failed
  left: 9
 right: 10
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
  21: ruff_linter::rules::pep8_naming::rules::invalid_first_argument_name::rename_parameter
             at ./ruff/crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs:267:24
  22: ruff_linter::rules::pep8_naming::rules::invalid_first_argument_name::invalid_first_argument_name::{{closure}}
             at ./ruff/crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs:237:9
  23: ruff_diagnostics::diagnostic::Diagnostic::try_set_optional_fix
             at ./ruff/crates/ruff_diagnostics/src/diagnostic.rs:67:15
  24: ruff_linter::rules::pep8_naming::rules::invalid_first_argument_name::invalid_first_argument_name
             at ./ruff/crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs:236:5
  25: ruff_linter::checkers::ast::analyze::deferred_scopes::deferred_scopes
             at ./ruff/crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs:436:17
  26: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2315:5
  27: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:156:40
  28: ruff_linter::linter::lint_fix
             at ./ruff/crates/ruff_linter/src/linter.rs:550:22
  29: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:280:14
  30: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:194:9
  31: std::panicking::try::do_call
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:554:40
  32: std::panicking::try
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:518:19
  33: std::panic::catch_unwind
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panic.rs:142:14
  34: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  35: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:193:18
  36: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:94:17
  37: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  38: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  39: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  40: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  41: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  42: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  43: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  44: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  45: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  46: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  47: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  48: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  49: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  50: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:165:10
  51: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:422:13
  52: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:199:33
  53: ruff::main
             at ./ruff/crates/ruff/src/main.rs:65:11
  54: core::ops::function::FnOnce::call_once
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/ops/function.rs:250:5
  55: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/sys_common/backtrace.rs:155:18
  56: std::rt::lang_start::{{closure}}
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/rt.rs:166:18
  57: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/ops/function.rs:284:13
  58: std::panicking::try::do_call
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:554:40
  59: std::panicking::try
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:518:19
  60: std::panic::catch_unwind
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panic.rs:142:14
  61: std::rt::lang_start_internal::{{closure}}
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/rt.rs:148:48
  62: std::panicking::try::do_call
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:554:40
  63: std::panicking::try
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:518:19
  64: std::panic::catch_unwind
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panic.rs:142:14
  65: std::rt::lang_start_internal
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/rt.rs:148:20
  66: main
  67: <unknown>
  68: __libc_start_main
  69: _start

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/15184860/python_compressed.zip)


---

_Label `bug` added by @AlexWaygood on 2024-05-02 07:33_

---

_Label `fuzzer` added by @AlexWaygood on 2024-05-02 07:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-02 17:00_

---

_Comment by @charliermarsh on 2024-05-02 17:11_

This is just a debug assertion.

---

_Referenced in [astral-sh/ruff#11249](../../astral-sh/ruff/pulls/11249.md) on 2024-05-02 17:28_

---

_Closed by @charliermarsh on 2024-05-02 17:59_

---

_Closed by @charliermarsh on 2024-05-02 17:59_

---
