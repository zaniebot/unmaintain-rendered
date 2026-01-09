---
number: 3717
title: "Deprecated `arg_enum` depends on removed `_clap_count_exprs`"
type: issue
state: closed
author: bgilbert
labels:
  - C-bug
assignees: []
created_at: 2022-05-10T18:46:54Z
updated_at: 2022-05-10T20:18:05Z
url: https://github.com/clap-rs/clap/issues/3717
synced_at: 2026-01-07T13:12:19-06:00
---

# Deprecated `arg_enum` depends on removed `_clap_count_exprs`

---

_Issue opened by @bgilbert on 2022-05-10 18:46_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.60.0 (Fedora 1.60.0-1.fc35)

### Clap Version

3.1.17

### Minimal reproducible code

```rust
use clap::arg_enum;

arg_enum! {
    pub enum Column {
        A,
        B,
        C,
    }
}

fn main() {}
```


### Steps to reproduce the bug with the above code

```
cargo build
```

### Actual Behaviour

```
error[E0433]: failed to resolve: could not find `_clap_count_exprs` in `$crate`
 --> src/main.rs:3:1  |
3 | / arg_enum! {
4 | |     pub enum Column {
5 | |         A,
6 | |         B,
7 | |         C,
8 | |     }
9 | | }
  | |_^ could not find `_clap_count_exprs` in `$crate`
  |
  = note: this error originates in the macro `$crate::arg_enum` (in Nightly builds, run with -Z macro-backtrace for more info)
```

### Expected Behaviour

Code compiles.

### Additional Context

c8254918d60ff552d5e881a8e8391e3127299804 re-added `arg_enum`, which uses `_clap_count_exprs`, but in the meantime `_clap_count_exprs` had been removed by https://github.com/clap-rs/clap/pull/1970.

### Debug Output

Same as above.

---

_Label `C-bug` added by @bgilbert on 2022-05-10 18:46_

---

_Referenced in [clap-rs/clap#3718](../../clap-rs/clap/pulls/3718.md) on 2022-05-10 19:43_

---

_Closed by @epage on 2022-05-10 20:18_

---
