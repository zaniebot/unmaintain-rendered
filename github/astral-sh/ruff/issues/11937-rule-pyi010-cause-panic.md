---
number: 11937
title: Rule PYI010 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - parser
  - fuzzer
assignees: []
created_at: 2024-06-19T10:31:37Z
updated_at: 2024-06-20T10:57:37Z
url: https://github.com/astral-sh/ruff/issues/11937
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule PYI010 cause panic

---

_Issue opened by @qarmin on 2024-06-19 10:31_


ruff 0.4.9+27 (cdc7c7144 2024-06-19) (latest changes from main branch)
```
ruff  *.py --select PYI010 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
        f"""
{            user_task_name: user-inputted `task_name` argument
        """
```

error
```
All checks passed!

error: Panicked while linting /opt/tmp_folder/694327410525594475.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_text_size/src/range.rs:48:9:
assertion failed: start.raw <= end.raw
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/alloc/src/boxed.rs:2034:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:783:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:649:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/sys_common/backtrace.rs:171:18
   5: rust_begin_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:645:5
   6: core::panicking::panic_fmt
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/panicking.rs:72:14
   7: core::panicking::panic
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/panicking.rs:145:5
   8: ruff_python_index::indexer::Indexer::from_tokens
   9: ruff_linter::linter::lint_fix
             at ./ruff/crates/ruff_linter/src/linter.rs:544:23
  10: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:274:14
  11: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:192:9
  12: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  13: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  14: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  15: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  16: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:191:18
  17: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:93:17
  18: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  19: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  20: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  21: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  22: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  23: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  24: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  25: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  26: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  27: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  28: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  29: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  30: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  31: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:163:10
  32: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:429:13
  33: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:202:33
  34: ruff::main
             at ./ruff/crates/ruff/src/main.rs:65:11
  35: core::ops::function::FnOnce::call_once
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/ops/function.rs:250:5
  36: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/sys_common/backtrace.rs:155:18
  37: std::rt::lang_start::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:166:18
  38: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/ops/function.rs:284:13
  39: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  40: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  41: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  42: std::rt::lang_start_internal::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:148:48
  43: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  44: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  45: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  46: std::rt::lang_start_internal
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:148:20
  47: main
  48: <unknown>
  49: __libc_start_main
  50: _start

```


[python_compressed.zip](https://github.com/user-attachments/files/15899437/python_compressed.zip)


---

_Comment by @dhruvmanila on 2024-06-19 11:16_

Hmm, that's unfortunate. This is occurring because the f-string is triple-quoted which means the tokens can span across multiple lines and thus the lexer can't really go back. #11916 would probably be quite useful here.

---

_Label `bug` added by @dhruvmanila on 2024-06-19 11:16_

---

_Referenced in [astral-sh/ruff#11938](../../astral-sh/ruff/issues/11938.md) on 2024-06-19 11:19_

---

_Comment by @MichaReiser on 2024-06-19 11:35_

Hmm, should the re-lexing logic even run if we're in a multiline f-string context?

---

_Comment by @dhruvmanila on 2024-06-19 11:55_

> Hmm, should the re-lexing logic even run if we're in a multiline f-string context?

I think we can recover by reducing the nesting but the lexer can't go back.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-06-19 11:56_

---

_Label `parser` added by @dhruvmanila on 2024-06-19 11:57_

---

_Label `fuzzer` added by @dhruvmanila on 2024-06-19 11:57_

---

_Comment by @MichaReiser on 2024-06-19 11:58_

Thanks @qarmin for all the fuzzing work. This has helped a lot to find some more obscure edge cases around the parser's new error recovery!

---

_Referenced in [astral-sh/ruff#11939](../../astral-sh/ruff/pulls/11939.md) on 2024-06-19 12:20_

---

_Closed by @dhruvmanila on 2024-06-20 10:57_

---

_Closed by @dhruvmanila on 2024-06-20 10:57_

---
