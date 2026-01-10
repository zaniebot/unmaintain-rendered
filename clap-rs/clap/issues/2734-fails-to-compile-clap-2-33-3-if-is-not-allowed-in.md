---
number: 2734
title: "fails to compile clap 2.33.3 -- `'if' is not allowed in a 'const fn'`"
type: issue
state: closed
author: jtmoon79
labels:
  - C-bug
assignees: []
created_at: 2021-08-22T03:37:29Z
updated_at: 2021-08-31T10:02:09Z
url: https://github.com/clap-rs/clap/issues/2734
synced_at: 2026-01-10T01:27:24Z
---

# fails to compile clap 2.33.3 -- `'if' is not allowed in a 'const fn'`

---

_Issue opened by @jtmoon79 on 2021-08-22 03:37_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.42.1

Specifically `rustc` version `1.41.1` and `cargo` version `1.42.1`.

### Clap Version

2.33.3

### Minimal reproducible code

`src/main.rs`
```rust
// src/main.rs
fn main() {}
```

`Cargo.toml`
```toml
[package]
name = "cargo1"
version = "0.0.1"

[dependencies]
clap = "2.33.3"
```

### Steps to reproduce the bug with the above code

`cargo build`

### Actual Behaviour

```
$ cargo build
   Compiling clap v2.33.3
error[E0658]: `if` is not allowed in a `const fn`
  --> /home/user1/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/settings.rs:7:1
   |
7  | / bitflags! {
8  | |     struct Flags: u64 {
9  | |         const SC_NEGATE_REQS       = 1;
10 | |         const SC_REQUIRED          = 1 << 1;
...  |
51 | |     }
52 | | }
   | |_^
   |
   = note: for more information, see https://github.com/rust-lang/rust/issues/49146
   = note: this error originates in a macro outside of the current crate (in Nightly builds, run with -Z external-macro-backtrace for more info)

error[E0658]: `if` is not allowed in a `const fn`
  --> /home/user1/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/args/settings.rs:6:1
   |
6  | / bitflags! {
7  | |     struct Flags: u32 {
8  | |         const REQUIRED         = 1;
9  | |         const MULTIPLE         = 1 << 1;
...  |
28 | |     }
29 | | }
   | |_^
   |
   = note: for more information, see https://github.com/rust-lang/rust/issues/49146
   = note: this error originates in a macro outside of the current crate (in Nightly builds, run with -Z external-macro-backtrace for more info)

error: aborting due to 2 previous errors

For more information about this error, try `rustc --explain E0658`.
error: could not compile `clap`.
```

### Expected Behaviour

```
$ cargo build

   Compiling cargo1 v0.0.1 (/tmp/project1)
    Finished dev [unoptimized + debuginfo] target(s) in 2.34s
```

### Additional Context

Run on Debian 9 Linux.

### Debug Output

No difference in `cargo build` output.

---

_Label `T: bug` added by @jtmoon79 on 2021-08-22 03:37_

---

_Renamed from "fails to compile clap 2.33.3 -- `if` is not allowed in a `const fn`" to "fails to compile clap 2.33.3 -- `'if' is not allowed in a 'const fn'`" by @jtmoon79 on 2021-08-22 03:42_

---

_Renamed from "fails to compile clap 2.33.3 -- `'if' is not allowed in a 'const fn'`" to "fails to build clap 2.33.3 -- `'if' is not allowed in a 'const fn'`" by @jtmoon79 on 2021-08-22 03:43_

---

_Renamed from "fails to build clap 2.33.3 -- `'if' is not allowed in a 'const fn'`" to "fails to compile clap 2.33.3 -- `'if' is not allowed in a 'const fn'`" by @jtmoon79 on 2021-08-22 03:43_

---

_Comment by @jtmoon79 on 2021-08-22 03:49_

The same minimal reproduction setup _passes_ when using `cargo` version `1.51.0` and `rustc` version `1.51.0` on Ubuntu 20.04.

The versions reported `1.42.1` and `1.41.1` are the supported versions in Debian 9 which is [supported by Debian until July 2022](https://wiki.debian.org/LTS).

---

_Comment by @epage on 2021-08-23 13:29_

This is a duplicate of #2691.  Users are subject to how the dependency resolver selects all crates and how that impacts MSRV.  By default, it will pick the latest for everything, so with a new `bitflags` changing its MSRV, people are now picking that up.  The flag `-Z minimal-versions` is an option to get around this but the challenge is not everyone tests with it, so people will have too-low of dependencies.     The other option is to patch dependencies.

What clap shouldn't do is constrain the dependency for everyone for the sake of MSRV.  That would be inaccurate and can lead to other problems like [what happened to nom](https://www.reddit.com/r/rust/comments/p8clcx/how_to_fix_cargo_dependency_issue/)

---

_Closed by @pksunkara on 2021-08-31 10:02_

---

_Referenced in [foresterre/cargo-msrv#312](../../foresterre/cargo-msrv/issues/312.md) on 2022-02-22 15:06_

---

_Referenced in [rcore-os/rCore-Tutorial#151](../../rcore-os/rCore-Tutorial/issues/151.md) on 2022-06-10 07:06_

---

_Referenced in [rust-lang/cargo#9930](../../rust-lang/cargo/issues/9930.md) on 2023-03-30 12:16_

---
