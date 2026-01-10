```yaml
number: 11752
title: Rule E501 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2024-06-05T12:42:54Z
updated_at: 2024-06-08T19:58:33Z
url: https://github.com/astral-sh/ruff/issues/11752
synced_at: 2026-01-10T11:09:53Z
```

# Rule E501 cause panic

---

_Issue opened by @qarmin on 2024-06-05 12:42_

ruff 0.4.7 (latest changes from main branch)
```
ruff  *.py --select E501 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
# https://stacoverflowÂom/questions/1674047/efficiòntway-to-find-mssingeements-in-an-integ
```

error
```
All checks passed!

error: Panicked while linting /opt/tmp_folder/10368904290721324990.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/rules/pycodestyle/overlong.rs:64:16:
attempt to subtract with overflow
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
   8: ruff_linter::rules::pycodestyle::overlong::Overlong::try_from_line
             at ./ruff/crates/ruff_linter/src/rules/pycodestyle/overlong.rs:64:16
   9: ruff_linter::rules::pycodestyle::rules::line_too_long::line_too_long
             at ./ruff/crates/ruff_linter/src/rules/pycodestyle/rules/line_too_long.rs:90:5
  10: ruff_linter::checkers::physical_lines::check_physical_lines
             at ./ruff/crates/ruff_linter/src/checkers/physical_lines.rs:60:39
  11: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:220:28
  12: ruff_linter::linter::lint_fix
             at ./ruff/crates/ruff_linter/src/linter.rs:560:22
  13: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:274:14
  14: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:192:9
  15: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  16: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  17: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  18: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  19: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:191:18
  20: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:93:17
  21: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  22: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  23: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  24: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  25: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  26: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  27: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  28: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  29: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  30: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  31: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  32: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  33: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  34: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:163:10
  35: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:429:13
  36: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:202:33
  37: ruff::main
             at ./ruff/crates/ruff/src/main.rs:65:11
  38: core::ops::function::FnOnce::call_once
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/ops/function.rs:250:5
  39: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/sys_common/backtrace.rs:155:18
  40: std::rt::lang_start::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:166:18
  41: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/ops/function.rs:284:13
  42: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  43: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  44: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  45: std::rt::lang_start_internal::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:148:48
  46: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  47: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  48: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  49: std::rt::lang_start_internal
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:148:20
  50: main
  51: <unknown>
  52: __libc_start_main
  53: _start

```
[python_compressed.zip](https://github.com/user-attachments/files/15587040/python_compressed.zip)



---

_Label `bug` added by @zanieb on 2024-06-05 12:59_

---

_Comment by @dhruvmanila on 2024-06-05 13:06_

I'm having a hard time reproducing this on the latest `main` (895eb3ef4) :(

Can you provide the exact commit you're on?

---

_Comment by @qarmin on 2024-06-05 13:22_

2e0a9755e07cfe7ee30683b65bbc1d9b5aa8b141 or 895eb3ef485836a75225e2534b638b98eaf30977

This is overflow panic, which is visible only in debug builds - in release version it wraps value.

I use this script in CI
```
  sed -i '/\[profile.release\]/a overflow-checks = true' Cargo.toml
  sed -i '/\[profile.release\]/a debug-assertions = true' Cargo.toml
  sed -i '/\[profile.release\]/a debug = true' Cargo.toml
  sd "MAX_ITERATIONS: usize = 100;" "MAX_ITERATIONS: usize = 500;" crates/ruff_linter/src/linter.rs
  rm rust-toolchain.toml
```

---

_Comment by @dhruvmanila on 2024-06-05 14:20_

Yeah, I'm using debug build

---

_Comment by @dhruvmanila on 2024-06-05 14:25_

I'm not able to reproduce this with the config you've provided either.

---

_Comment by @qarmin on 2024-06-05 15:53_

Well, I can reproduce it both in CI and locally with ruff 0.4.7+46 (895eb3ef4 2024-06-05) with release build
The problem happens only if app is installed via cargo install - no idea why 
```
git clone https://github.com/astral-sh/ruff.git
cd ruff
  sed -i '/\[profile.release\]/a overflow-checks = true' Cargo.toml
  sed -i '/\[profile.release\]/a debug-assertions = true' Cargo.toml
  sed -i '/\[profile.release\]/a debug = true' Cargo.toml
  sd "MAX_ITERATIONS: usize = 100;" "MAX_ITERATIONS: usize = 500;" crates/ruff_linter/src/linter.rs
  rm rust-toolchain.toml 
```

```
cargo build
target/debug/ruff check . --select E501 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
# no problems
cargo build --release
target/release/ruff check . --select E501 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
# no problems
```
```
cargo install --path crates/ruff/ --force
ruff check . --select E501 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
# crash
```

---

_Comment by @dhruvmanila on 2024-06-07 03:54_

Unfortunately, install ruff via `cargo install` works for me :(

```console
$ ~/.cargo/bin/ruff version                                                                                                           
ruff 0.4.7+46 (895eb3ef4 2024-06-05)

$ ~/.cargo/bin/ruff check --select E501 --no-cache --fix --unsafe-fixes --preview --output-format full --isolated ~/Downloads/panic.py   
/Users/dhruv/Downloads/panic.py:1:92: E501 Line too long (90 > 88)
  |
1 | # https://stacoverflowÂom/questions/1674047/efficiòntway-to-find-mssingeements-in-an-integ
  |                                                                                         ^^ E501
  |

Found 1 error.

```

---

_Comment by @qarmin on 2024-06-07 11:38_

Finally I found that this was caused by using non-locked dependencies
```
cargo install . # cause problems
cargo install . --locked # works fine
```
after 
```
cargo update
cargo build --release
```
I can reproduce problem - so feel free to close it, because it not happens with official version 

---

_Comment by @charliermarsh on 2024-06-08 19:58_

Thanks!

---

_Closed by @charliermarsh on 2024-06-08 19:58_

---

_Label `fuzzer` added by @charliermarsh on 2024-06-08 19:58_

---
