```yaml
number: 12158
title: ruff has some cache bug that causes crashes
type: issue
state: closed
author: hauntsaninja
labels:
  - bug
  - great writeup
assignees: []
created_at: 2024-07-03T00:15:59Z
updated_at: 2024-07-03T12:36:47Z
url: https://github.com/astral-sh/ruff/issues/12158
synced_at: 2026-01-12T15:54:51Z
```

# ruff has some cache bug that causes crashes

---

_@hauntsaninja_

Here's a directory structure that repros:
```
λ tree                        
.
└── dir
    ├── prefix
    │   ├── abc
    │   │   └── __init__.py
    │   └── pyproject.toml
    ├── prefixabc
    │   └── pyproject.toml
    └── pyproject.toml

5 directories, 4 files
```
Contents don't matter.

If you run ruff twice, you'll crash with:
```
warning: Different package root in cache: expected '/Users/shantanu/tmp/ruffcrash/dir/prefixabc', got '/Users/shantanu/tmp/ruffcrash/dir/prefix/abc'
error: Panicked while linting dir/prefixabc/pyproject.toml: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff/src/diagnostics.rs:195:18:
wrong package cache for file
Backtrace:    0: std::backtrace::Backtrace::force_capture
   1: std::backtrace::Backtrace::force_capture
   2: <ruff::panic::PanicError as core::fmt::Display>::fmt
   3: std::panicking::rust_panic_with_hook
   4: <std::panicking::begin_panic_handler::StaticStrPayload as core::panic::PanicPayload>::take_box
   5: <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt
   6: _rust_begin_unwind
   7: core::panicking::panic_fmt
   8: <core::panic::panic_info::PanicInfo as core::fmt::Display>::fmt
   9: core::option::expect_failed
  10: <ruff::diagnostics::FixMap as core::ops::arith::AddAssign>::add_assign
  11: <ruff::panic::PanicError as core::fmt::Display>::fmt
  12: ruff::check
  13: <ruff::version::VersionInfo as core::fmt::Display>::fmt
  14: <ruff::args::LogLevelArgs as clap_builder::derive::Args>::augment_args
  15: <ruff::panic::PanicError as core::fmt::Display>::fmt
  16: <ruff::args::LogLevelArgs as clap_builder::derive::Args>::augment_args
  17: <ruff::panic::PanicError as core::fmt::Display>::fmt
  18: ruff::check
  19: rayon_core::registry::WorkerThread::wait_until_cold
  20: rayon_core::registry::ThreadBuilder::run
  21: <rayon_core::unwind::AbortIfPanic as core::ops::drop::Drop>::drop
  22: <rayon_core::registry::WorkerThread as core::convert::From<rayon_core::registry::ThreadBuilder>>::from
  23: std::sys::pal::unix::thread::Thread::new
  24: __pthread_start
```

---

_Comment by @hauntsaninja on 2024-07-03 00:17_

```
mkdir dir
cd dir

mkdir prefixabc
touch prefixabc/pyproject.toml

mkdir prefix
touch prefix/pyproject.toml
mkdir prefix/abc
touch prefix/abc/__init__.py

cd ..
ruff check .
ruff check .
```

---

_Label `great writeup` added by @zanieb on 2024-07-03 00:19_

---

_Label `bug` added by @zanieb on 2024-07-03 00:19_

---

_Comment by @zanieb on 2024-07-03 00:24_

I'm very suspicious of our `CacheKey` implementation at

https://github.com/astral-sh/ruff/blob/f9214f95bb8fae17822981c95a9a8549035bace2/crates/ruff_cache/src/cache_key.rs#L350-L355

The standard library implementation we call appears to skip separators? https://doc.rust-lang.org/src/std/path.rs.html#3083-3085

Seems weird it'd treat these paths as the same when hashing though?

---

_Comment by @zanieb on 2024-07-03 00:27_

I successfully reproduced the panic (thank you so so much for the easy script) and resolved it with

```diff
diff --git a/crates/ruff_cache/src/cache_key.rs b/crates/ruff_cache/src/cache_key.rs
index 1208c4010..710be29c6 100644
--- a/crates/ruff_cache/src/cache_key.rs
+++ b/crates/ruff_cache/src/cache_key.rs
@@ -350,7 +350,7 @@ impl<K: CacheKey + Ord, V: CacheKey> CacheKey for BTreeMap<K, V> {
 impl CacheKey for Path {
     #[inline]
     fn cache_key(&self, state: &mut CacheKeyHasher) {
-        self.hash(&mut *state);
+        self.to_string_lossy().hash(&mut *state);
     }
 }
 ```

---

_Comment by @zanieb on 2024-07-03 00:33_

Ah yeesh and here's a confirmation https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=467c0f7aedcfbfb484c9ca1ec65ff406

cc @BurntSushi is this.. intentional?

---

_Comment by @MichaReiser on 2024-07-03 06:25_

Uh that's nasty, but technically the `Hash` implementation of rustc is correct because the only contract of `Hash` is 

```
k1 == k2 -> hash(k1) == hash(k2)
```

This is fine in hash maps where the next step is to call `k1.eq(k2)`. IMO, rolling our own `CacheKey` implementation is the correct solution.

---

_Closed by @zanieb on 2024-07-03 12:36_

---

_Closed by @zanieb on 2024-07-03 12:36_

---
