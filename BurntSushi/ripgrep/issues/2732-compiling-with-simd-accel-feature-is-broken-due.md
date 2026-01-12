```yaml
number: 2732
title: compiling with simd-accel feature is broken due to removal of stdsimd feature in nightly breaking the packed_simd crate
type: issue
state: closed
author: R-Goc
labels: []
assignees: []
created_at: 2024-02-10T23:03:38Z
updated_at: 2024-02-10T23:09:14Z
url: https://github.com/BurntSushi/ripgrep/issues/2732
synced_at: 2026-01-12T16:13:24Z
```

# compiling with simd-accel feature is broken due to removal of stdsimd feature in nightly breaking the packed_simd crate

---

_@R-Goc_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

14.1

### How did you install ripgrep?

Cargo 
cargo +nightly install --force --profile "release" --features 'simd-accel' ripgrep

### What operating system are you using ripgrep on?

Windows, though it affects all systems

### Describe your bug.

The crate fails to compile with feature 'simd-accel' due to feature stdsimd being removed which broke packed_simd crate used for the 'simd-accel' feature. No impact on ripgrep otherwise. Only compiling with the feature isn't possible. 
The commit that broke packed-simd is: https://github.com/rust-lang/rust/commit/ea37e8091fe87ae0a7e204c034e7d55061e56790
Relevant tracking issues: https://github.com/rust-lang/rust/issues/27731 and https://github.com/rust-lang/rust/issues/48556

### What are the steps to reproduce the behavior?

Installing or compiling ripgrep with 'simd-accel'  for example with:
cargo +nightly install --force --profile "release" --features 'simd-accel' ripgrep
target-cpu=native also has to be set. 


### What is the actual behavior?

cargo +nightly install --force --profile "release" --features 'simd-accel' ripgrep
    Updating crates.io index
  Installing ripgrep v14.1.0
    Updating crates.io index
  Downloaded packed_simd v0.3.9
  Downloaded num-traits v0.2.18
  Downloaded 2 crates (150.3 KB) in 0.58s
   Compiling memchr v2.7.1
   Compiling winapi v0.3.9
   Compiling libm v0.2.8
   Compiling regex-syntax v0.8.2
   Compiling autocfg v1.1.0
   Compiling packed_simd v0.3.9
   Compiling log v0.4.20
   Compiling cfg-if v1.0.0
   Compiling serde v1.0.196
   Compiling crossbeam-utils v0.8.19
   Compiling num-traits v0.2.18
   Compiling aho-corasick v1.1.2
   Compiling grep-matcher v0.1.7
   Compiling serde_json v1.0.113
   Compiling itoa v1.0.10
   Compiling ryu v1.0.16
   Compiling memmap2 v0.9.4
   Compiling crossbeam-epoch v0.9.18
   Compiling anyhow v1.0.79
   Compiling winapi-util v0.1.6
   Compiling crossbeam-deque v0.8.5
   Compiling ripgrep v14.1.0
   Compiling termcolor v1.4.1
   Compiling same-file v1.0.6
   Compiling walkdir v2.4.0
   Compiling textwrap v0.16.0
   Compiling lexopt v0.3.0
   Compiling regex-automata v0.4.5
error[E0635]: unknown feature `stdsimd`
   --> \.cargo\registry\src\index.crates.io-6f17d22bba15001f\packed_simd-0.3.9\src\lib.rs:219:5
    |
219 |     stdsimd,
    |     ^^^^^^^

For more information about this error, try `rustc --explain E0635`.
error: could not compile `packed_simd` (lib) due to 1 previous error

### What is the expected behavior?

should compile with the feature enabled.

---

_Comment by @BurntSushi on 2024-02-10 23:09_

I don't think this needs to be tracked here. It's a nightly feature anyway and will get fixed once packed_simd is fixed.

---

_Closed by @BurntSushi on 2024-02-10 23:09_

---
