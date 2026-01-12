```yaml
number: 942
title: Attempting to install with --features unstable
type: issue
state: closed
author: ianks
labels:
  - question
assignees: []
created_at: 2018-06-12T14:57:14Z
updated_at: 2018-06-12T15:00:19Z
url: https://github.com/BurntSushi/ripgrep/issues/942
synced_at: 2026-01-12T16:13:22Z
```

# Attempting to install with --features unstable

---

_@ianks_

Based on the README, I am trying to install ripgrep through cargo to get simd accelleration, but am met with this error:
```
cargo install ripgrep --features unstable
    Updating registry `https://github.com/rust-lang/crates.io-index`
 Downloading ripgrep v0.8.1                                                     
  Installing ripgrep v0.8.1                                                     
error: failed to compile `ripgrep v0.8.1`, intermediate artifacts can be found at `/tmp/cargo-installhiwOF5`

Caused by:
  Package `ripgrep v0.8.1` does not have these features: `unstable`
```

Not sure if I am doing something wrong, or if the readme needs to be updated.

```
rustc --version
rustc 1.28.0-nightly (1d4dbf488 2018-06-11)

cargo --version
cargo 1.28.0-nightly (e2348c2db 2018-06-07)
```

---

_Comment by @BurntSushi on 2018-06-12 14:59_

The README is correct, but it refers to current master, which isn't released. If you want installation instructions for `0.8.1` (including how to enable SIMD), then see the [README for 0.8.1](https://github.com/BurntSushi/ripgrep/tree/0.8.1).

---

_Closed by @BurntSushi on 2018-06-12 14:59_

---

_Label `question` added by @BurntSushi on 2018-06-12 14:59_

---

_Comment by @BurntSushi on 2018-06-12 15:00_

(You could also do `cargo install --git https://github.com/BurntSushi/ripgrep` I think, and the current README will work.)

---
