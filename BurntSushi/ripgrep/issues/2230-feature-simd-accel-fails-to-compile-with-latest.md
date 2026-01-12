```yaml
number: 2230
title: feature simd-accel fails to compile with latest nightly
type: issue
state: closed
author: underclocked
labels: []
assignees: []
created_at: 2022-06-10T13:26:00Z
updated_at: 2022-06-10T13:39:51Z
url: https://github.com/BurntSushi/ripgrep/issues/2230
synced_at: 2026-01-12T16:13:24Z
```

# feature simd-accel fails to compile with latest nightly

---

_@underclocked_

#### What version of ripgrep are you using?

HEAD

#### How did you install ripgrep?

Issue building

#### What operating system are you using ripgrep on?

macOS 12.3.1

#### Describe your bug.

ripgrep fails to compile with the latest rust nightly build with:

```
error: expected one of `!` or `::`, found keyword `mod`
   --> /Users/guerrera/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.7/src/lib.rs:347:7
    |
347 | crate mod llvm {
    |       ^^^ expected one of `!` or `::`

error: could not compile `packed_simd_2` due to previous error
```
This seems to be related to 

https://github.com/rust-lang/packed_simd/issues/343

#### What are the steps to reproduce the behavior?

```
> rustup update
> git clone https://github.com/BurntSushi/ripgrep
> cd ripgrep
> RUSTFLAGS="-C target-cpu=native" cargo +nightly build --release --features 'pcre2 simd-accel'
```

#### What is the actual behavior?

Build fails (see above)

#### What is the expected behavior?

build completes successfully

---

_Closed by @BurntSushi on 2022-06-10 13:39_

---
