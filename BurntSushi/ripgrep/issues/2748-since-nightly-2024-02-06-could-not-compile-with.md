```yaml
number: 2748
title: "Since nightly-2024-02-06 , could not compile with --features 'simd-accel'"
type: issue
state: closed
author: PierreAntoineGuillaume
labels: []
assignees: []
created_at: 2024-03-07T13:44:24Z
updated_at: 2024-03-07T15:13:13Z
url: https://github.com/BurntSushi/ripgrep/issues/2748
synced_at: 2026-01-12T16:13:24Z
```

# Since nightly-2024-02-06 , could not compile with --features 'simd-accel'

---

_@PierreAntoineGuillaume_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

6ebebb2 (current master HEAD, few commits after 14.1.0)


### How did you install ripgrep?

Trying to compiling it from source

### What operating system are you using ripgrep on?

Ubuntu 20.04.6 LTS

### Describe your bug.

Cannot compile

### What are the steps to reproduce the behavior?

RUSTUP_TOOLCHAIN=nightly-2024-02-06 cargo build --features 'simd-accel' fails
RUSTUP_TOOLCHAIN=nightly-2024-02-05 cargo build --features 'simd-accel' works


Because: 
```shell
   --> ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/packed_simd-0.3.9/src/lib.rs:218:5
    |
218 |     platform_intrinsics,
    |     ^^^^^^^^^^^^^^^^^^^ feature has been removed
    |
    = note: SIMD intrinsics use the regular intrinsics ABI now
```

Then in later nightly version :

```
invalid ABI: found `platform-intrinsic`
```

related: https://github.com/rust-lang/rust/commit/ea37e8091fe87ae0a7e204c034e7d55061e56790
https://github.com/rust-lang/rust/pull/117372


I don't know if this is actually a bug.



### What is the actual behavior?

Cannot compile

### What is the expected behavior?

Can compile

---

_Comment by @PierreAntoineGuillaume on 2024-03-07 13:45_

This is more a report and a reference than a real bug, and I will be trying to make a pull request but anything simd-related is way above my knowledge.

---

_Comment by @BurntSushi on 2024-03-07 14:24_

As documented in the README, `simd-accel` requires a Rust nightly compiler because it relies on unstable features. As also documented in the README, the _only_ thing it helps is UTF-16 decoding. So you might want to re-evaluate why you're enabling it in the first place.

I am considering just removing the `simd-accel` feature because I am absolutely tired of the fact that `encoding_rs` refuses to use stable APIs for its SIMD features. All of ripgrep's SIMD optimizations for search do not require it. They use stable but platform specific APIs.

In any case, I'm not tracking this issue here. There's an issue for it at the source: https://github.com/rust-lang/packed_simd/issues/359

---

_Closed by @BurntSushi on 2024-03-07 14:24_

---

_Closed by @BurntSushi on 2024-03-07 14:47_

---

_Comment by @BurntSushi on 2024-03-07 14:48_

I ended up just removing `simd-accel` altogether because I'm tired of dealing with it. See https://github.com/BurntSushi/ripgrep/pull/2749

---

_Comment by @PierreAntoineGuillaume on 2024-03-07 15:13_

Really nice !

Thanks for your time, and for your tools too.

---
