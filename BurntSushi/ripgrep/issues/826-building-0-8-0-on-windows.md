```yaml
number: 826
title: Building 0.8.0 on windows
type: issue
state: closed
author: albfan
labels:
  - invalid
  - question
assignees: []
created_at: 2018-02-20T14:00:43Z
updated_at: 2018-02-20T17:12:53Z
url: https://github.com/BurntSushi/ripgrep/issues/826
synced_at: 2026-01-12T16:13:22Z
```

# Building 0.8.0 on windows

---

_@albfan_

#### What version of ripgrep are you using? 

0.8.0

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your question, feature request, or bug.

Cannot compile

#### If this is a bug, what are the steps to reproduce the behavior?

```
$ cargo build --release
 Downloading clap v2.30.0
   Compiling regex-syntax v0.4.2
   Compiling bitflags v1.0.1
   Compiling crossbeam v0.3.2
   Compiling fnv v1.0.6
error: expected ident, found #
   --> C:\Users\afanjul\.cargo\registry\src\github.com-1ecc6299db9ec823\bitflags-1.0.1\src\lib.rs:423:29
    |
423 |                               #[allow(deprecated)]
    |                               ^
    |
   ::: C:\Users\afanjul\.cargo\registry\src\github.com-1ecc6299db9ec823\bitflags-1.0.1\src\example_generated.rs
    |
4   | / bitflags! {
5   | |     /// This is the same `Flags` struct defined in the [crate level example](../index.html#example).
6   | |     /// Note that this struct is just for documentation purposes only, it must not be used outside
7   | |     /// this crate.
...   |
13  | |     }
14  | | }
    | |_- in this macro invocation
```

$ rustup.exe --version
rustup 1.11.0 (e751ff9f8 2018-02-13)

$ rustc.exe --version
rustc 1.20.0-nightly (ab5bec255 2017-06-22)

$ cargo --version
cargo 0.21.0-nightly (50b1c24d1 2017-06-17)

#### If this is a bug, what is the actual behavior?

compilation fails

#### If this is a bug, what is the expected behavior?

compile rg

---

_Comment by @BurntSushi on 2018-02-20 14:04_

> rustc 1.20.0-nightly (ab5bec255 2017-06-22)

That's your likely problem. This is an *ancient* nightly. While ripgrep's README should be more clear on the matter, the only plausible choice is to follow whatever the latest Rust nightly is, and this is what everyone else does anyway. Please upgrade your Rust nightly (or switch to Rust stable >= 1.20) and try again.

---

_Comment by @albfan on 2018-02-20 14:06_

Sorry to ask. How do I update that?

cargo update

rustup-init <whatever>

I try here https://www.rust-lang.org/es-ES/install.html but nothing happens after it says (you're updated)

---

_Comment by @BurntSushi on 2018-02-20 14:08_

If you're managing your installation with rustup then you should use rustup. Please see the documentation: https://github.com/rust-lang-nursery/rustup.rs#keeping-rust-up-to-date

---

_Comment by @albfan on 2018-02-20 17:08_

$ rustup update

Thanks for the support

---

_Closed by @albfan on 2018-02-20 17:08_

---

_Label `invalid` added by @BurntSushi on 2018-02-20 17:12_

---

_Label `question` added by @BurntSushi on 2018-02-20 17:12_

---
