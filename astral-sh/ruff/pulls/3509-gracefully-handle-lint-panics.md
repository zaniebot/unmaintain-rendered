```yaml
number: 3509
title: Gracefully handle lint panics
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - cli
assignees: []
merged: true
base: main
head: feat/gracefully-handle-lint-panics
created_at: 2023-03-14T12:50:00Z
updated_at: 2023-03-19T16:08:40Z
url: https://github.com/astral-sh/ruff/pull/3509
synced_at: 2026-01-12T15:55:13Z
```

# Gracefully handle lint panics

---

_@MichaReiser_

This PR wraps the linting of every file in a [`catch_unwind`](https://blog.rust-lang.org/2016/05/26/Rust-1.9.html#controlled-unwinding) to catch potential panics (due to a bug in ruff) and prints a warning instead of crashing ruff. Catching panics on a per-file basis has the advantage that it doesn't prevent users from adopting or continuing using Ruff because of a bug triggered by a specific code snipped. The catch handler also includes the name of the problematic file to ease identifying the bug. 

The main downside of this approach is that we now need to compile ruff with `panic=unwind` instead of `panic=abort`. Changing the panic type results in a ~15% performance regression on my machine:

```
Panic 'unwind': ./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache
  Time (mean ± σ):     215.3 ms ±   6.2 ms    [User: 3753.3 ms, System: 98.0 ms]
  Range (min … max):   203.8 ms … 226.1 ms    13 runs
 
Panic 'abort': ./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache
  Time (mean ± σ):     188.9 ms ±   3.6 ms    [User: 3216.8 ms, System: 116.1 ms]
  Range (min … max):   181.5 ms … 194.0 ms    15 runs
```

It also slightly increases the binary from ~19MB to ~20MB due to the additional metadata required to enable unwinding (stack walking etc).

## Test Plan

I changed a linter and added an intentional index out of bounds. Ruff linted all files but printed the following warning everytime the out of bounds index paniced

```
warning: Linting panicked /home/micha/.config/JetBrains/CLion2022.3/scratches/scratch_3.py: This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BLinter%20panic%5D

with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'index out of bounds: the len is 0 but the index is 1', crates/ruff/src/rules/pycodestyle/rules/compound_statements.rs:108:14
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
             at ./crates/ruff_cli/src/panic.rs:35:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/alloc/src/boxed.rs:2032:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:692:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:579:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/sys_common/backtrace.rs:137:18
   5: rust_begin_unwind
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:575:5
   6: core::panicking::panic_fmt
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/panicking.rs:64:14
   7: core::panicking::panic_bounds_check
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/panicking.rs:147:5
   8: <usize as core::slice::index::SliceIndex<[T]>>::index
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/slice/index.rs:260:10
   9: core::slice::index::<impl core::ops::index::Index<I> for [T]>::index
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/slice/index.rs:18:9
  10: <alloc::vec::Vec<T,A> as core::ops::index::Index<I>>::index
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/alloc/src/vec/mod.rs:2727:9
  11: ruff::rules::pycodestyle::rules::compound_statements::compound_statements
             at ./crates/ruff/src/rules/pycodestyle/rules/compound_statements.rs:108:14
  12: ruff::checkers::tokens::check_tokens
             at ./crates/ruff/src/checkers/tokens.rs:123:13
  13: ruff::linter::check_path
             at ./crates/ruff/src/linter.rs:90:28
  14: ruff::linter::lint_only
             at ./crates/ruff/src/linter.rs:325:18
  15: ruff_cli::diagnostics::lint_path
             at ./crates/ruff_cli/src/diagnostics.rs:129:22
  16: ruff_cli::commands::run::lint_path::{{closure}}
             at ./crates/ruff_cli/src/commands/run.rs:146:9
  17: std::panicking::try::do_call
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:483:40
  18: __rust_try
  19: std::panicking::try
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:447:19
  20: std::panic::catch_unwind
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panic.rs:137:14
  21: ruff_cli::panic::catch_unwind
             at ./crates/ruff_cli/src/panic.rs:44:18
  22: ruff_cli::commands::run::lint_path
             at ./crates/ruff_cli/src/commands/run.rs:145:18
  23: ruff_cli::commands::run::run::{{closure}}
             at ./crates/ruff_cli/src/commands/run.rs:88:21
  24: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &F>::call_mut
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/ops/function.rs:593:13
  25: core::iter::adapters::map::map_fold::{{closure}}
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/iter/adapters/map.rs:84:28
  26: core::iter::traits::iterator::Iterator::fold
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/iter/traits/iterator.rs:2414:21
  27: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/iter/adapters/map.rs:124:9
  28: <rayon::iter::reduce::ReduceFolder<R,T> as rayon::iter::plumbing::Folder<T>>::consume_iter
             at /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/reduce.rs:105:19
  29: <rayon::iter::map::MapFolder<C,F> as rayon::iter::plumbing::Folder<T>>::consume_iter
             at /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/map.rs:248:21
  30: rayon::iter::plumbing::Producer::fold_with
             at /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:110:9
  31: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:438:13
  32: rayon::iter::plumbing::bridge_producer_consumer
             at /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:397:12
  33: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:373:13
  34: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/slice/mod.rs:732:9
  35: rayon::iter::plumbing::bridge
             at /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:357:12
  36: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/slice/mod.rs:708:9
  37: <rayon::iter::map::Map<I,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/map.rs:49:9
  38: rayon::iter::reduce::reduce
             at /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/reduce.rs:15:5
  39: rayon::iter::ParallelIterator::reduce
             at /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/mod.rs:991:9
  40: ruff_cli::commands::run::run
             at ./crates/ruff_cli/src/commands/run.rs:76:40
  41: ruff_cli::check
             at ./crates/ruff_cli/src/lib.rs:244:13
  42: ruff_cli::run
             at ./crates/ruff_cli/src/lib.rs:83:40
  43: ruff::main
             at ./crates/ruff_cli/src/bin/ruff.rs:45:11
  44: core::ops::function::FnOnce::call_once
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/ops/function.rs:507:5
  45: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/sys_common/backtrace.rs:121:18
  46: std::rt::lang_start::{{closure}}
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/rt.rs:166:18
  47: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/ops/function.rs:606:13
  48: std::panicking::try::do_call
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:483:40
  49: std::panicking::try
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:447:19
  50: std::panic::catch_unwind
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panic.rs:137:14
  51: std::rt::lang_start_internal::{{closure}}
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/rt.rs:148:48
  52: std::panicking::try::do_call
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:483:40
  53: std::panicking::try
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:447:19
  54: std::panic::catch_unwind
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panic.rs:137:14
  55: std::rt::lang_start_internal
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/rt.rs:148:20
  56: std::rt::lang_start
             at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/rt.rs:165:17
  57: main
  58: <unknown>
  59: __libc_start_main
  60: _start
```

The top frames are related to Rust's unwind handling but frame 11 points to the problematic line. 


## Alternatives

* We could set a panic handler (`set_handler`) per processed file. This would allow us to print the file name before crashing ruff, but it doesn't solve the problem that the bug now prevents users from using ruff. 
* Keep as is
* Ship two binaries and fallback to the binary with `catch_unwind` enabled if we run into a panic :rofl: 

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/run.rs`:172 on 2023-03-14 14:27_

We would, ideally, push a warning `Diagnostic` instead of returning `Ok` (which is decisive and could result in miscounts if we ever print all linted files), but our diagnostic infrastructure doesn't allow this today.  

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:49 on 2023-03-14 14:27_

Any reasons for only enabling this handler in release builds?

---

_Review requested from @charliermarsh by @MichaReiser on 2023-03-14 14:31_

---

_Label `performance` added by @MichaReiser on 2023-03-14 14:31_

---

_Label `cli` added by @MichaReiser on 2023-03-14 14:31_

---

_Marked ready for review by @MichaReiser on 2023-03-14 14:31_

---

_@MichaReiser reviewed on 2023-03-16 10:52_

---

_Comment by @github-actions[bot] on 2023-03-16 11:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.08ms     2.5 MB/sec    1.00     16.6±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.02ms     3.8 MB/sec    1.00      4.3±0.01ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.03    601.8±2.20µs     4.9 MB/sec    1.00    586.9±1.77µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.03ms     3.5 MB/sec    1.00      7.3±0.03ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      9.0±0.04ms     4.5 MB/sec    1.00      8.9±0.03ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.0±0.01ms     8.3 MB/sec    1.00   1978.3±3.63µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    230.5±0.67µs    12.8 MB/sec    1.00    227.5±1.61µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.2±0.01ms     6.1 MB/sec    1.00      4.1±0.02ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.0±0.52ms     2.5 MB/sec    1.00     15.9±0.59ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.3±0.21ms     3.9 MB/sec    1.00      4.2±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   549.6±25.25µs     5.4 MB/sec    1.00   540.8±18.87µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.1±0.30ms     3.6 MB/sec    1.00      6.9±0.17ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.04      8.8±0.24ms     4.6 MB/sec    1.00      8.5±0.34ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1937.6±72.40µs     8.6 MB/sec    1.00  1892.3±96.19µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.01   215.2±12.72µs    13.7 MB/sec    1.00    212.7±8.49µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.1±0.14ms     6.3 MB/sec    1.00      3.9±0.19ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-03-17 19:29_

How do you think about the tradeoffs here? Would we ever want to enable this until we feel really confident in no-panics, and then remove?

---

_Comment by @charliermarsh on 2023-03-17 19:30_

(Also, the benchmarks look unchanged -- any idea why that is?)

---

_Comment by @MichaReiser on 2023-03-17 20:37_

> (Also, the benchmarks look unchanged -- any idea why that is?)

Cargo doesn't support 'panic=abort' for benchmarks or tests, they always run with 'panic=unwind' [[source]](https://github.com/rust-lang/cargo/issues/11214)

I'll reply to your other question tomorrow.

---

_Comment by @MichaReiser on 2023-03-18 09:15_

> How do you think about the tradeoffs here? Would we ever want to enable this until we feel really confident in no-panics, and then remove?

It's a difficult one. It comes comes down to the assessment of whether Ruff aborting in the case of a panic is a problem for users. 

In my view, the value of Ruff is increased productivity for users, allowing them to ship high-quality software faster. And ruff delivers on that promise for many users by reducing the lint time from 30s+ to a few milliseconds. But, in my view, it's less important for productivity whether Ruff takes 190ms or 220ms, it doesn't significantly impact productivity. 

However, productivity takes a significant hit if Ruff blows up because of a bug, because then:

* users have to do without Ruff's guidance to write high-quality software
* either users spend (a lot of) time identifying the bug and, in the best case, suppress the problematic rule in the file or on the line causing the bug
* ..or users disable ruff until the fix is shipped (locally, on CI, for everyone)

That's why I'm leaning toward making this a permanent change, except if we have the data that panics are extremely rare, we're in a position to ship bugfixes in hours to days, and Ruff is an established reliable high-quality productivity tool. But @charliermarsh, you're in a better position to assess if this has been a pain-point for users or if I'm extravagating ;) 

Obviously, regressing performance by 15% is significant. Especially because it applies to all code, including the code and tool we'll write in the future. My take here is that I'm convinced that we can improve performance by more than 15% if we are mindful of performance (make performance a habit) and focus on performant architectures and algorithms. Unfortunately, this requires more effort than using `panic=abort`. 


---

_@charliermarsh approved on 2023-03-18 18:26_

I agree with your assessment, thanks for spelling it out so clearly.

---

_Comment by @charliermarsh on 2023-03-18 18:28_

For what it's worth, I'm not sure how to quantify it, but I do think panics are uncommon. Most panic reports have fallen into two camps: (1) some file in the codebase uses CR line endings, typically a file that exists to _test_ CR line endings (we improved this recently); or (2) actual fuzzing. But every panic _has_ been indicative of a real bug, and _any_ file panicking makes Ruff totally unusable right now.


---

_Comment by @MichaReiser on 2023-03-19 12:27_

Current dependencies on/for this PR:
* main
  * **PR #3509** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3509" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/charliermarsh/ruff/3509?utm_source=stack-comment).

---

_Merged by @MichaReiser on 2023-03-19 16:08_

---

_Closed by @MichaReiser on 2023-03-19 16:08_

---

_Branch deleted on 2023-03-19 16:08_

---
