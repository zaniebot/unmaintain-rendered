```yaml
number: 3299
title: "Can`t build on linux gnu"
type: issue
state: closed
author: nrot
labels:
  - C-bug
assignees: []
created_at: 2022-01-16T21:27:47Z
updated_at: 2022-01-17T13:51:27Z
url: https://github.com/clap-rs/clap/issues/3299
synced_at: 2026-01-12T16:14:14Z
```

# Can`t build on linux gnu

---

_@nrot_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.53.0

### Clap Version

3.0.7

### Minimal reproducible code

```rust
use clap::{App, arg};

fn main() {
    println!("Hello, world!");
    let args = App::new("").args(&[
        arg!(<FILE> "file to show"),
        arg!(--filter [filter] "filter type: Nearest,Triangle,CatmullRom,Gaussian,Lanczos3"),
        arg!(-s --"scale_font" [scale_font] "scale of font to correct image"),
    ]).get_matches();
    println!("File: {:?}", args.value_of("FILE"))
}

```


### Steps to reproduce the bug with the above code

On wsl inside Windows 10.
> uname -a
Linux HOME-PC 5.10.16.3-microsoft-standard-WSL2 #1 SMP Fri Apr 2 22:23:49 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
> cargo build


### Actual Behaviour

cargo build                                                                                
   Compiling libc v0.2.112
   Compiling memchr v2.4.1
   Compiling indexmap v1.8.0
   Compiling os_str_bytes v6.0.0
   Compiling atty v0.2.14
   Compiling clap v3.0.7
error[E0658]: arbitrary expressions in key-value attributes are unstable
 --> /home/nrot/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.7/src/lib.rs:8:39
  |
8 | #![cfg_attr(feature = "derive", doc = include_str!("../README.md"))]
  |                                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = note: see issue #78835 <https://github.com/rust-lang/rust/issues/78835> for more information

error: aborting due to previous error

For more information about this error, try `rustc --explain E0658`.
error: could not compile `clap`

To learn more, run the command again with --verbose.

### Expected Behaviour

The build app

### Additional Context

rustup show

Default host: x86_64-unknown-linux-gnu
rustup home:  /home/nrot/.rustup

installed toolchains
--------------------

stable-x86_64-unknown-linux-gnu (default)
nightly-x86_64-unknown-linux-gnu

active toolchain
----------------

stable-x86_64-unknown-linux-gnu (default)
rustc 1.58.0 (02072b482 2022-01-11

### Debug Output

error[E0658]: arbitrary expressions in key-value attributes are unstable
  --> /home/nrot/.cargo/registry/src/github.com-1ecc6299db9ec823/clap_derive-3.0.6/src/lib.rs:16:10
   |
16 | #![doc = include_str!("../README.md")]
   |          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = note: see issue #78835 <https://github.com/rust-lang/rust/issues/78835> for more information

error: aborting due to previous error

For more information about this error, try `rustc --explain E0658`.
error: could not compile `clap_derive`

To learn more, run the command again with --verbose.

---

_Label `C-bug` added by @nrot on 2022-01-16 21:27_

---

_Comment by @epage on 2022-01-17 13:51_

Our documented minimum-supported Rust version (or MSRV) is [1.54.0](https://github.com/clap-rs/clap#about) which is when "arbitrary expressions in key-value attributes" was stabilized.

I'd recommend upgrading Rust.  If there is a problem with doing so, I'd recommend continuing this discussion on https://github.com/clap-rs/clap/issues/3267.

---

_Closed by @epage on 2022-01-17 13:51_

---
