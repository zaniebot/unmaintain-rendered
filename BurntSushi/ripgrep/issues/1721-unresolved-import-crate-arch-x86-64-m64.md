```yaml
number: 1721
title: "unresolved import `crate::arch::x86_64::__m64`"
type: issue
state: closed
author: jerryc05
labels:
  - bug
assignees: []
created_at: 2020-11-01T10:28:18Z
updated_at: 2020-11-02T15:52:54Z
url: https://github.com/BurntSushi/ripgrep/issues/1721
synced_at: 2026-01-12T16:13:24Z
```

# unresolved import `crate::arch::x86_64::__m64`

---

_@jerryc05_

Just a short note, `packed_simd` has a known break on Windows 10. Please see this issue: https://github.com/rust-lang/packed_simd/issues/308

The workaround is not so simple. We might have to modify the version of `encoding_rs` dependency to `encoding_rs = { git = "https://github.com/hsivonen/encoding_rs" }` in both these files:
- `crates/searcher/Cargo.toml`
- `encoding_rs_io` src in cargo cache


The error log of `RUSTFLAGS="-C target-cpu=native" cargo build --release --features 'simd-accel'` is:
```
error[E0432]: unresolved import `crate::arch::x86_64::__m64`
  --> /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/api/into_bits/arch_specific.rs:51:15
   |
51 |               $($arch_ty),*
   |                 ^^^^^^^^ no `__m64` in `arch::x86_64`
...
86 | / impl_arch!(
87 | |     [x86["x86"]: __m64], [x86_64["x86_64"]: __m64],
88 | |     [arm["arm"]: int8x8_t, uint8x8_t, poly8x8_t, int16x4_t, uint16x4_t,
89 | |      poly16x4_t, int32x2_t, uint32x2_t, float32x2_t, int64x1_t,
...  |
96 | |     test: test_v64
97 | | );
   | |__- in this macro invocation
   |
   = note: this error originates in a macro (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0432]: unresolved import `crate::arch::x86_64::_mm_movemask_pi8`
  --> /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/reductions/mask/x86/sse.rs:47:21
   |
47 |                 use crate::arch::x86_64::_mm_movemask_pi8;
   |                     ^^^^^^^^^^^^^^^^^^^^^----------------
   |                     |                    |
   |                     |                    help: a similar name exists in the module: `_mm_movemask_epi8`
   |                     no `_mm_movemask_pi8` in `arch::x86_64`
   |
  ::: /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/reductions/mask.rs:41:1
   |
41 | impl_mask_reductions!(m8x8);
   | ---------------------------- in this macro invocation
   |
   = note: this error originates in a macro (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0432]: unresolved import `crate::arch::x86_64::_mm_movemask_pi8`
  --> /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/reductions/mask/x86/sse.rs:62:21
   |
62 |                 use crate::arch::x86_64::_mm_movemask_pi8;
   |                     ^^^^^^^^^^^^^^^^^^^^^----------------
   |                     |                    |
   |                     |                    help: a similar name exists in the module: `_mm_movemask_epi8`
   |                     no `_mm_movemask_pi8` in `arch::x86_64`
   |
  ::: /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/reductions/mask.rs:41:1
   |
41 | impl_mask_reductions!(m8x8);
   | ---------------------------- in this macro invocation
   |
   = note: this error originates in a macro (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0432]: unresolved import `crate::arch::x86_64::_mm_movemask_pi8`
  --> /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/reductions/mask/x86/sse.rs:47:21
   |
47 |                 use crate::arch::x86_64::_mm_movemask_pi8;
   |                     ^^^^^^^^^^^^^^^^^^^^^----------------
   |                     |                    |
   |                     |                    help: a similar name exists in the module: `_mm_movemask_epi8`
   |                     no `_mm_movemask_pi8` in `arch::x86_64`
   |
  ::: /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/reductions/mask.rs:47:1
   |
47 | impl_mask_reductions!(m16x4);
   | ----------------------------- in this macro invocation
   |
   = note: this error originates in a macro (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0432]: unresolved import `crate::arch::x86_64::_mm_movemask_pi8`
  --> /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/reductions/mask/x86/sse.rs:62:21
   |
62 |                 use crate::arch::x86_64::_mm_movemask_pi8;
   |                     ^^^^^^^^^^^^^^^^^^^^^----------------
   |                     |                    |
   |                     |                    help: a similar name exists in the module: `_mm_movemask_epi8`
   |                     no `_mm_movemask_pi8` in `arch::x86_64`
   |
  ::: /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/reductions/mask.rs:47:1
   |
47 | impl_mask_reductions!(m16x4);
   | ----------------------------- in this macro invocation
   |
   = note: this error originates in a macro (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0432]: unresolved import `crate::arch::x86_64::_mm_movemask_pi8`
  --> /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/reductions/mask/x86/sse.rs:47:21
   |
47 |                 use crate::arch::x86_64::_mm_movemask_pi8;
   |                     ^^^^^^^^^^^^^^^^^^^^^----------------
   |                     |                    |
   |                     |                    help: a similar name exists in the module: `_mm_movemask_epi8`
   |                     no `_mm_movemask_pi8` in `arch::x86_64`
   |
  ::: /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/reductions/mask.rs:52:1
   |
52 | impl_mask_reductions!(m32x2);
   | ----------------------------- in this macro invocation
   |
   = note: this error originates in a macro (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0432]: unresolved import `crate::arch::x86_64::_mm_movemask_pi8`
  --> /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/reductions/mask/x86/sse.rs:62:21
   |
62 |                 use crate::arch::x86_64::_mm_movemask_pi8;
   |                     ^^^^^^^^^^^^^^^^^^^^^----------------
   |                     |                    |
   |                     |                    help: a similar name exists in the module: `_mm_movemask_epi8`
   |                     no `_mm_movemask_pi8` in `arch::x86_64`
   |
  ::: /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/reductions/mask.rs:52:1
   |
52 | impl_mask_reductions!(m32x2);
   | ----------------------------- in this macro invocation
   |
   = note: this error originates in a macro (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0432]: unresolved import `crate::arch::x86_64::_mm_shuffle_pi8`
   --> /home/***/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/codegen/shuffle1_dyn.rs:40:29
    |
40  |                         use crate::arch::x86_64::_mm_shuffle_pi8;
    |                             ^^^^^^^^^^^^^^^^^^^^^---------------
    |                             |                    |
    |                             |                    help: a similar name exists in the module: `_mm_shuffle_epi8`
    |                             no `_mm_shuffle_pi8` in `arch::x86_64`
...
297 | impl_shuffle1_dyn!(u8x8);
    | ------------------------- in this macro invocation
    |
    = note: this error originates in a macro (in Nightly builds, run with -Z macro-backtrace for more info)
```

---

_Comment by @BurntSushi on 2020-11-01 15:44_

This isn't really in ripgrep's power to fix. @hsivonen has to publish a new release of `encoding_rs` with [this PR](https://github.com/hsivonen/encoding_rs/pull/58), which was merged a bit ago.

---

_Comment by @jerryc05 on 2020-11-01 20:03_

Yeah I understand. I am just posting this here for reference BTW.

---

_Comment by @hsivonen on 2020-11-02 08:08_

I've published `encoding_rs` 0.8.25 with the PR mentioned above.

I noticed that even before the semver break to 1.0, `cfg-if` did an MSRV increase without a semver break (when updating to edition 2018), so there's no point in holding back `encoding_rs` changes out of MSRV increase concern.

---

_Comment by @BurntSushi on 2020-11-02 08:34_

Thanks! And yeah, I thought you stopped tracking MSRV stuff a while back. I did the same for ripgrep. (But still do it for some libraries.)

---

_Label `bug` added by @BurntSushi on 2020-11-02 14:56_

---

_Closed by @BurntSushi on 2020-11-02 15:52_

---
