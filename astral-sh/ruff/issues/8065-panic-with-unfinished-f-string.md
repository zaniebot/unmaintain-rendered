```yaml
number: 8065
title: Panic with unfinished f-string
type: issue
state: closed
author: konstin
labels:
  - bug
  - linter
assignees: []
created_at: 2023-10-19T15:10:28Z
updated_at: 2023-10-27T11:11:45Z
url: https://github.com/astral-sh/ruff/issues/8065
synced_at: 2026-01-12T15:54:47Z
```

# Panic with unfinished f-string

---

_@konstin_

Running ruff with
```python
f"{123}
```
(note the missing closing quote) panics:
```
$ ruff scratch.py 
error: Panicked while linting scratch.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'assertion failed: self.start_locations.is_empty()', crates/ruff_python_index/src/fstring_ranges.rs:92:9
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
             at ./crates/ruff_cli/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/alloc/src/boxed.rs:2007:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:709:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:595:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/sys_common/backtrace.rs:151:18
   5: rust_begin_unwind
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:593:5
   6: core::panicking::panic_fmt
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/core/src/panicking.rs:67:14
   7: core::panicking::panic
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/core/src/panicking.rs:117:5
   8: ruff_python_index::fstring_ranges::FStringRangesBuilder::finish
             at ./crates/ruff_python_index/src/fstring_ranges.rs:92:9
   9: ruff_python_index::indexer::Indexer::from_tokens
             at ./crates/ruff_python_index/src/indexer.rs:75:29
  10: ruff_linter::linter::lint_only
             at ./crates/ruff_linter/src/linter.rs:387:19
  11: ruff_cli::diagnostics::lint_path
             at ./crates/ruff_cli/src/diagnostics.rs:325:22
  12: ruff_cli::commands::check::lint_path::{{closure}}
             at ./crates/ruff_cli/src/commands/check.rs:237:9
  13: std::panicking::try::do_call
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:500:40
  14: __rust_try
  15: std::panicking::try
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:464:19
  16: std::panic::catch_unwind
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panic.rs:142:14
  17: ruff_cli::panic::catch_unwind
             at ./crates/ruff_cli/src/panic.rs:40:18
  18: ruff_cli::commands::check::lint_path
             at ./crates/ruff_cli/src/commands/check.rs:236:18
  19: ruff_cli::commands::check::check::{{closure}}
             at ./crates/ruff_cli/src/commands/check.rs:134:17
  20: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:123:36
  21: rayon::iter::plumbing::Folder::consume_iter
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:179:20
  22: rayon::iter::plumbing::Producer::fold_with
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:110:9
  23: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:438:13
  24: rayon::iter::plumbing::bridge_producer_consumer
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:397:12
  25: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:373:13
  26: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/slice/mod.rs:732:9
  27: rayon::iter::plumbing::bridge
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:357:12
  28: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/slice/mod.rs:708:9
  29: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:46:9
  30: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/fold.rs:59:9
  31: rayon::iter::reduce::reduce
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/reduce.rs:15:5
  32: rayon::iter::ParallelIterator::reduce
             at /home/konsti/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/mod.rs:991:9
  33: ruff_cli::commands::check::check
             at ./crates/ruff_cli/src/commands/check.rs:198:48
  34: ruff_cli::check
             at ./crates/ruff_cli/src/lib.rs:373:13
  35: ruff_cli::run
             at ./crates/ruff_cli/src/lib.rs:162:33
  36: ruff::main
             at ./crates/ruff_cli/src/bin/ruff.rs:49:11
  37: core::ops::function::FnOnce::call_once
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/core/src/ops/function.rs:250:5
  38: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/sys_common/backtrace.rs:135:18
  39: std::rt::lang_start::{{closure}}
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/rt.rs:166:18
  40: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/core/src/ops/function.rs:284:13
  41: std::panicking::try::do_call
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:500:40
  42: std::panicking::try
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:464:19
  43: std::panic::catch_unwind
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panic.rs:142:14
  44: std::rt::lang_start_internal::{{closure}}
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/rt.rs:148:48
  45: std::panicking::try::do_call
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:500:40
  46: std::panicking::try
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:464:19
  47: std::panic::catch_unwind
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panic.rs:142:14
  48: std::rt::lang_start_internal
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/rt.rs:148:20
  49: std::rt::lang_start
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/rt.rs:165:17
  50: main
  51: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  52: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  53: _start
```

---

_Label `bug` added by @konstin on 2023-10-19 15:10_

---

_Label `parser` added by @konstin on 2023-10-19 15:10_

---

_Comment by @charliermarsh on 2023-10-19 15:13_

\cc @dhruvmanila 

---

_Comment by @charliermarsh on 2023-10-19 17:18_

I guess this is arguably an "incorrect" debug assertion? 🤔 

---

_Comment by @dhruvmanila on 2023-10-19 17:36_

> I guess this is arguably an "incorrect" debug assertion? 🤔

Probably. The lexer works as it'll give out the error token stating that the string is unterminated but we flatten the token iterator (`tokens.iter().flatten()`) while constructing the index.

But, then if we remove the assertion, the ranges could potentially be incomplete. For instance, the above example means that there's a `FStringStart` token but there won't be a range corresponding to that f-string in the index. We rely on the guarantee that the ranges will be present if we encounter any of the f-string tokens (using `unwrap`).

The reason to use `unwrap` is that a single f-string is made up of multiple tokens but we need the entire f-string range when we encounter certain tokens within the f-string (`FStringStart` / `FStringMiddle`).

We could potentially avoid unwrapping by skipping the check for that f-string which seems like the correct behavior. That change will be dependent on where the ranges are being asked for.


---

_Label `parser` removed by @dhruvmanila on 2023-10-24 04:25_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-10-24 04:26_

---

_Comment by @dhruvmanila on 2023-10-24 04:26_

(Removed the "parser" label because the problem is with the `Indexer`)

---

_Label `linter` added by @MichaReiser on 2023-10-24 04:39_

---

_Comment by @dhruvmanila on 2023-10-24 04:42_

### Possible solutions:

1. Remove the debug assertion. This means that:
	* There could be unfinished f-strings in the source code for which the lexer would emit `FStringStart` and possibly `FStringMiddle` tokens but the indexer won't contain those ranges.
	* And, all of the `unwrap` while getting the f-string range from the indexer needs to account for the `None` variant as well.
2. Once all the tokens are consumed, the remaining f-string start locations (unterminated f-strings) should use the source code end location by default.
	* Here, the indexer will contain the ranges for unterminated f-strings as well but it ends at the last character in the source code.

The previous behavior (without PEP 701 support) matched (1) so I'll go with that.

---

_Closed by @dhruvmanila on 2023-10-27 11:11_

---
