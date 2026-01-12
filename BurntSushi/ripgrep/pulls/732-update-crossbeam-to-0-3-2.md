```yaml
number: 732
title: Update crossbeam to 0.3.2
type: pull_request
state: merged
author: ghost
labels: []
assignees: []
merged: true
base: master
head: update-crossbeam
created_at: 2018-01-06T12:31:36Z
updated_at: 2018-01-06T14:37:23Z
url: https://github.com/BurntSushi/ripgrep/pull/732
synced_at: 2026-01-12T18:23:13Z
```

# Update crossbeam to 0.3.2

---

_@ghost_

Crossbeam 0.3.0 uses the old auto trait syntax and 0.3.2 switched to the new one. Ripgrep is a part of Rust's test suite and, since it depends on crossbeam 0.3.0, it is currently blocking the following PR, which removes the old auto trait syntax in favor of the new one: https://github.com/rust-lang/rust/pull/46480#issuecomment-355735400

Diff between versions 0.3.0 and 0.3.2: https://github.com/crossbeam-rs/crossbeam/compare/v0.3.0...v0.3.2

---

_Merged by @BurntSushi on 2018-01-06 14:05_

---

_Closed by @BurntSushi on 2018-01-06 14:05_

---

_Comment by @BurntSushi on 2018-01-06 14:06_

@stjepang Cool! Do you pin to a specific commit on ripgrep, or should I prioritize a new release?

---

_Branch deleted on 2018-01-06 14:16_

---

_Comment by @ghost on 2018-01-06 14:22_

Rust's test suite pins to a specific commit, so we're covered there.

But I still suggest releasing a new version because crossbeam 0.3.0 will not compile with nightly rustc as soon as https://github.com/rust-lang/rust/pull/46480 gets merged. Even worse, I'm afraid it won't compile with stable rustc in the future either (from version 1.25 onwards).
  

---

_Comment by @BurntSushi on 2018-01-06 14:30_

So that means newer versions of Rust won't be able to compile older versions of ripgrep? That doesn't sound good to me.

---

_Comment by @BurntSushi on 2018-01-06 14:31_

@stjepang Anyway, thanks so much for the heads up! I'm glad this won't catch me by surprise. :) I will see if I can get the full story at rust-lang/rust.

---

_Comment by @ghost on 2018-01-06 14:37_

Yes, the problem is that the following will cause a compilation error, even if you disable the `nightly` feature. The reason is that this syntactic construction errors during the parsing stage, even before the compiler gets to handling attributes.

```rust
#[cfg(feature = "nightly")]
unsafe impl ZerosValid for .. {}
```
  

---
