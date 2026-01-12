```yaml
number: 1175
title: "simd-accel no longer works (encoding_rs's SIMD support is broken on the latest nightly)"
type: issue
state: closed
author: lilydjwg
labels: []
assignees: []
created_at: 2019-01-24T07:21:57Z
updated_at: 2019-02-07T22:06:13Z
url: https://github.com/BurntSushi/ripgrep/issues/1175
synced_at: 2026-01-12T16:13:23Z
```

# simd-accel no longer works (encoding_rs's SIMD support is broken on the latest nightly)

---

_@lilydjwg_

#### What version of ripgrep are you using?

N/A.

#### How did you install ripgrep?

Failed.

#### What operating system are you using ripgrep on?

I'm trying to build on Arch Linux with Intel(R) Xeon(R) CPU E5-2620 v3 and I'm going to run it on Arch Linux with Intel(R) Core(TM) i7-7700HQ.

#### If this is a bug, what are the steps to reproduce the behavior?

I'm building with:

```
RUSTFLAGS="-C target-cpu=skylake" cargo build --release --features 'simd-accel pcre2'
```

#### If this is a bug, what is the actual behavior?

I got a lot of errors like this:

```
error: unrecognized platform-specific intrinsic function: `x86_mm_movemask_ps`      
  --> /build/.cargo/registry/src/github.com-1ecc6299db9ec823/simd-0.2.3/src/x86/sse2.rs:10:5
(more omitted)
```

#### If this is a bug, what is the expected behavior?

There shouldn't be such errors.

----

It seems that cargo is trying to build the build script with options intended for the target system.

rustc and cargo are nightly verions.

---

_Closed by @BurntSushi on 2019-01-24 12:01_

---

_Renamed from "failed to build if the build system doesn't support target features" to "encoding_rs's SIMD support is broken on the latest nightly" by @BurntSushi on 2019-01-24 12:01_

---

_Comment by @BurntSushi on 2019-01-24 12:06_

This has nothing to do with whether the build system supports the prescribed target features. The issue is that `encoding_rs`'s SIMD support is now broken on the latest release of nightly Rust. The only way it becomes unbroken is if nightly Rust reverts the change removing unstable legacy support for for platform specific intrinsics, or if `encoding_rs` migrates to the new SIMD infrastructure. The latest is, AFAIK, tracked by https://github.com/hsivonen/encoding_rs/issues/23.

I've updated the README in https://github.com/BurntSushi/ripgrep/commit/9a9f54d44ce45c6c3c5bfa31ad1a08fda57cdad7 to note that ripgrep's `simd-accel` feature is broken.

---

_Comment by @lilydjwg on 2019-01-24 12:58_

I see, thank you.

---

_Renamed from "encoding_rs's SIMD support is broken on the latest nightly" to "simd-accel no longer works (encoding_rs's SIMD support is broken on the latest nightly)" by @BurntSushi on 2019-01-24 13:24_

---

_Comment by @hsivonen on 2019-02-07 19:38_

It works now if the lock file is updated with `cargo update` first. The README for `encoding_rs` has the details of the pitfalls of trying to build for 32-bit ARM targets whose first component does not have the substring `neon`.

---

_Comment by @BurntSushi on 2019-02-07 22:06_

@hsivonen Awesome! Thanks so much. `Cargo.lock` and `README` have been updated!

---
