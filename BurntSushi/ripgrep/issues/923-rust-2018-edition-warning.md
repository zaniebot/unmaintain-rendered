```yaml
number: 923
title: Rust 2018 edition warning
type: issue
state: closed
author: sophiajt
labels:
  - bug
assignees: []
created_at: 2018-05-20T19:00:39Z
updated_at: 2018-06-24T00:49:08Z
url: https://github.com/BurntSushi/ripgrep/issues/923
synced_at: 2026-01-12T16:13:22Z
```

# Rust 2018 edition warning

---

_@sophiajt_

I just noticed that if you build ripgrep with a recent Rust nightly, you get a big warning at the top:

```
warning: An explicit [[test]] section is specified in Cargo.toml which currently
disables Cargo from automatically inferring other test targets.
This inference behavior will change in the Rust 2018 edition and the following
files will be included as a test target:

* C:\Users\Jonathan\source\ripgrep\tests\hay.rs
* C:\Users\Jonathan\source\ripgrep\tests\workdir.rs

This is likely to break cargo build or cargo test as these files may not be
ready to be compiled as a test target today. You can future-proof yourself
and disable this warning by adding `autotests = false` to your [package]
section. You may also move the files to a location where Cargo would not
automatically infer them to be a target, such as in subfolders.

For more information on this warning you can consult
https://github.com/rust-lang/cargo/issues/5330
````


#### If this is a bug, what are the steps to reproduce the behavior?

Build ripgrep with a recent Rust nightly



---

_Comment by @BurntSushi on 2018-05-20 19:05_

Thanks! I saw this and have it fixed on my dev branch. :-) It's good to track this though to make sure it gets done.

---

_Label `bug` added by @BurntSushi on 2018-05-20 19:06_

---

_Comment by @BurntSushi on 2018-05-20 19:06_

(We should just set `autotests = false`.)

---

_Closed by @BurntSushi on 2018-06-24 00:49_

---
