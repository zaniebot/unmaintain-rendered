---
number: 13755
title: Checking file with rule FIX001 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2024-10-15T04:51:25Z
updated_at: 2024-10-15T08:49:54Z
url: https://github.com/astral-sh/ruff/issues/13755
synced_at: 2026-01-10T01:22:54Z
---

# Checking file with rule FIX001 cause panic

---

_Issue opened by @qarmin on 2024-10-15 04:51_

ruff 0.6.9+947 (04b636cba 2024-10-14)
```
ruff check *.py --select FIX001 --no-cache  --preview --output-format concise --isolated
```

file content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
# #d#
```

error
```
All checks passed!

error: Panicked while linting /tmp/tmp_folder/data/8372306581891477414.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/directives.rs:319:33:
byte index 4 is out of bounds of `#d#`
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/alloc/src/boxed.rs:2468:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/std/src/panicking.rs:809:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/std/src/panicking.rs:674:13
   4: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/std/src/sys/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/std/src/panicking.rs:665:5
   6: core::panicking::panic_fmt
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/core/src/panicking.rs:74:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/core/src/str/mod.rs:68:5
   9: core::str::traits::<impl core::slice::index::SliceIndex<str> for core::ops::range::RangeFrom<usize>>::index
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/str/traits.rs:537:21
  10: core::str::traits::<impl core::ops::index::Index<I> for str>::index
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/str/traits.rs:60:15
  11: ruff_linter::directives::TodoDirective::from_comment
             at ./ruff/crates/ruff_linter/src/directives.rs:319:33
  12: ruff_linter::directives::TodoComment::from_comment
             at ./ruff/crates/ruff_linter/src/directives.rs:266:9
  13: ruff_linter::checkers::tokens::check_tokens::{{closure}}
             at ./ruff/crates/ruff_linter/src/checkers/tokens.rs:184:17
  14: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:294:13
  15: core::iter::traits::iterator::Iterator::find_map::check::{{closure}}
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2903:32
  16: <core::iter::adapters::enumerate::Enumerate<I> as core::iter::traits::iterator::Iterator>::try_fold::enumerate::{{closure}}
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/enumerate.rs:86:27
  17: core::iter::traits::iterator::Iterator::try_fold
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2406:21
  18: <core::iter::adapters::enumerate::Enumerate<I> as core::iter::traits::iterator::Iterator>::try_fold
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/enumerate.rs:92:9
  19: core::iter::traits::iterator::Iterator::find_map
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2909:9
  20: <core::iter::adapters::filter_map::FilterMap<I,F> as core::iter::traits::iterator::Iterator>::next
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/filter_map.rs:64:9
  21: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter_nested.rs:24:32
  22: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/vec/spec_from_iter.rs:33:9
  23: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:3388:9
  24: core::iter::traits::iterator::Iterator::collect
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2001:9
  25: ruff_linter::checkers::tokens::check_tokens
             at ./ruff/crates/ruff_linter/src/checkers/tokens.rs:186:14
  26: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:93:28
  27: ruff_linter::linter::lint_only
             at ./ruff/crates/ruff_linter/src/linter.rs:409:23
  28: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:321:22
  29: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:192:9
  30: std::panicking::try::do_call
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:557:40
  31: __rust_try
  32: std::panicking::try
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:520:19
  33: std::panic::catch_unwind
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
  34: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  35: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:191:18
  36: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:93:17
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
             at ./ruff/crates/ruff/src/commands/check.rs:163:10
  51: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:416:13
  52: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:188:33
  53: ruff::main
             at ./ruff/crates/ruff/src/main.rs:87:11
  54: core::ops::function::FnOnce::call_once
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  55: std::sys::backtrace::__rust_begin_short_backtrace
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:154:18
  56: std::rt::lang_start::{{closure}}
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:195:18
  57: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/core/src/ops/function.rs:284:13
  58: std::panicking::try::do_call
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/std/src/panicking.rs:557:40
  59: std::panicking::try
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/std/src/panicking.rs:520:19
  60: std::panic::catch_unwind
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/std/src/panic.rs:358:14
  61: std::rt::lang_start_internal::{{closure}}
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/std/src/rt.rs:174:48
  62: std::panicking::try::do_call
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/std/src/panicking.rs:557:40
  63: std::panicking::try
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/std/src/panicking.rs:520:19
  64: std::panic::catch_unwind
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/std/src/panic.rs:358:14
  65: std::rt::lang_start_internal
             at /rustc/27861c429af736ea3a6bb015956c7286071b286d/library/std/src/rt.rs:174:20
  66: std::rt::lang_start
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:194:17
  67: <unknown>
  68: __libc_start_main
  69: _start

```

Ruff build, that was used to reproduce problem(compiled on Ubuntu 22.04 with relase mode + debug symbols + debug assertions + overflow checks) - https://github.com/qarmin/Automated-Fuzzer/releases/download/Nightly/ruff.7z

[python_compressed.zip](https://github.com/user-attachments/files/17372730/python_compressed.zip)

also other rules panics, but I think that with exactly same reason:

[python_compressed.zip](https://github.com/user-attachments/files/17372745/python_compressed.zip)
[python_compressed.zip](https://github.com/user-attachments/files/17372746/python_compressed.zip)


---

_Referenced in [astral-sh/ruff#13756](../../astral-sh/ruff/pulls/13756.md) on 2024-10-15 06:26_

---

_Comment by @MichaReiser on 2024-10-15 06:28_

You're quick at finding bugs. This code landed less then 24h ago :) Which is great, because I now still have all relevant context to fix the bug. Thank you!

---

_Label `bug` added by @dhruvmanila on 2024-10-15 06:58_

---

_Closed by @MichaReiser on 2024-10-15 08:49_

---
