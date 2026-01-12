```yaml
number: 18859
title: "Rule `manual_dict_comprehension` cause panic `for-loop target binding must exist` (probably PERF403 - debug assert)"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2025-06-22T13:29:04Z
updated_at: 2025-06-25T08:44:34Z
url: https://github.com/astral-sh/ruff/issues/18859
synced_at: 2026-01-12T15:54:56Z
```

# Rule `manual_dict_comprehension` cause panic `for-loop target binding must exist` (probably PERF403 - debug assert)

---

_@qarmin_

### Summary

File content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
for o,(x,)in():v[x,]=o
```

command
```
timeout -v 300 ruff check TEST___FILE.py --select ALL --preview --output-format concise --no-cache --fix --unsafe-fixes --isolated
```

App was compiled with nightly rust compiler to be able to use address sanitizer
(You can ignore this part if there is no address sanitizer error)
On Ubuntu 24.04, the commands to compile were:
```
rustup default nightly
rustup component add rust-src --toolchain nightly-x86_64-unknown-linux-gnu
rustup component add llvm-tools-preview --toolchain nightly-x86_64-unknown-linux-gnu

export RUST_BACKTRACE=1 # or full depending on project
export ASAN_SYMBOLIZER_PATH=$(which llvm-symbolizer-18)
export ASAN_OPTIONS=symbolize=1
RUSTFLAGS="-Zsanitizer=address" cargo +nightly build --target x86_64-unknown-linux-gnu
```

cause this
```
All checks passed!

warning: `incorrect-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `incorrect-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
error: Panicked while linting /opt/BROKEN_FILES_DIR/926_IDX_0_RAND_553482853979823657879343_minimized_997.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs:149:9:
for-loop target binding must exist
Backtrace:    0: ruff_db::panic::install_hook::{{closure}}::{{closure}}
             at ./ruff-main/crates/ruff_db/src/panic.rs:91:34
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/alloc/src/boxed.rs:1980:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:841:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:699:13
   4: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/sys/backtrace.rs:168:18
   5: __rustc::rust_begin_unwind
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:697:5
   6: core::panicking::panic_fmt
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/core/src/panicking.rs:75:14
   7: ruff_linter::rules::perflint::rules::manual_dict_comprehension::manual_dict_comprehension::{{closure}}
             at ./ruff-main/crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs:149:9
   8: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::any
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/slice/iter/macros.rs:308:24
   9: ruff_linter::rules::perflint::rules::manual_dict_comprehension::manual_dict_comprehension
             at ./ruff-main/crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs:180:29
  10: ruff_linter::checkers::ast::analyze::deferred_for_loops::deferred_for_loops
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/analyze/deferred_for_loops.rs:39:17
  11: ruff_linter::checkers::ast::check_ast
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/mod.rs:3093:5
  12: ruff_linter::linter::check_path
             at ./ruff-main/crates/ruff_linter/src/linter.rs:229:39
  13: ruff_linter::linter::lint_fix
             at ./ruff-main/crates/ruff_linter/src/linter.rs:639:27
  14: ruff::diagnostics::lint_path
             at ./ruff-main/crates/ruff/src/diagnostics.rs:268:18
  15: ruff::commands::check::lint_path::{{closure}}
             at ./ruff-main/crates/ruff/src/commands/check.rs:192:9
  16: std::panicking::catch_unwind::do_call
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:589:40
  17: __rust_try
  18: std::panicking::catch_unwind
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:552:19
  19: std::panic::catch_unwind
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  20: ruff_db::panic::catch_unwind
             at ./ruff-main/crates/ruff_db/src/panic.rs:122:18
  21: ruff::commands::check::lint_path
             at ./ruff-main/crates/ruff/src/commands/check.rs:191:18
  22: ruff::commands::check::check::{{closure}}
             at ./ruff-main/crates/ruff/src/commands/check.rs:94:17
  23: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  24: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:25
  25: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:16
  26: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:22
  27: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  28: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  29: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:826:18
  30: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:21
  31: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:802:9
  32: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:46:19
  33: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/fold.rs:59:19
  34: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/reduce.rs:15:8
  35: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/mod.rs:998:9
  36: ruff::commands::check::check
             at ./ruff-main/crates/ruff/src/commands/check.rs:164:10
  37: ruff::check
             at ./ruff-main/crates/ruff/src/lib.rs:420:13
  38: ruff::run
             at ./ruff-main/crates/ruff/src/lib.rs:198:33
  39: ruff::main
             at ./ruff-main/crates/ruff/src/main.rs:45:11
  40: core::ops::function::FnOnce::call_once
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  41: std::sys::backtrace::__rust_begin_short_backtrace
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152:18
  42: std::rt::lang_start::{{closure}}
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:206:18
  43: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/core/src/ops/function.rs:284:21
  44: std::panicking::catch_unwind::do_call
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:589:40
  45: std::panicking::catch_unwind
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:552:19
  46: std::panic::catch_unwind
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panic.rs:359:14
  47: std::rt::lang_start_internal::{{closure}}
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/rt.rs:175:24
  48: std::panicking::catch_unwind::do_call
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:589:40
  49: std::panicking::catch_unwind
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:552:19
  50: std::panic::catch_unwind
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panic.rs:359:14
  51: std::rt::lang_start_internal
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/rt.rs:171:5
  52: std::rt::lang_start
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:205:5
  53: <unknown>
  54: __libc_start_main
  55: _start



##### Automatic Fuzzer note, output status "Some(0)", output signal "None"
```

[compressed.zip](https://github.com/user-attachments/files/20852923/compressed.zip)

### Version

90894932634fe58bd26bb7a586a969383bc49b29

---

_Label `bug` added by @ntBre on 2025-06-22 14:42_

---

_Label `fuzzer` added by @ntBre on 2025-06-22 14:42_

---

_Closed by @MichaReiser on 2025-06-25 08:44_

---
