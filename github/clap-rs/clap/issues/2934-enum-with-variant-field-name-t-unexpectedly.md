---
number: 2934
title: "`enum` with variant field `name: T` unexpectedly imposes `&mut T: PartialEq<&str>` in 3.0.0-beta.5"
type: issue
state: closed
author: ErichDonGubler
labels:
  - C-bug
assignees: []
created_at: 2021-10-23T16:36:12Z
updated_at: 2021-10-23T20:57:11Z
url: https://github.com/clap-rs/clap/issues/2934
synced_at: 2026-01-07T13:12:19-06:00
---

# `enum` with variant field `name: T` unexpectedly imposes `&mut T: PartialEq<&str>` in 3.0.0-beta.5

---

_Issue opened by @ErichDonGubler on 2021-10-23 16:36_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.56.0 (09c42c458 2021-10-18)

### Clap Version

3.0.0-beta.5

### Minimal reproducible code

`Cargo.toml`: 

```toml
[package]
name = "tmp-wfwrb0"
version = "0.1.0"
edition = "2021"

[dependencies]
clap = "=3.0.0-beta.5"
```

`src/main.rs`:

```rust
use clap::Parser;

// This works fine.
#[derive(Parser)]
struct Struct {
    #[clap(long)]
    name: bool,
}

// ...but this doesn't compile.
#[derive(Debug, Parser)]
pub enum Enum {
    Subcommand {
        #[clap(long)]
        name: bool,
    },
}

fn main() {}
```


### Steps to reproduce the bug with the above code

`cargo check` should be sufficient.

### Actual Behaviour

A compile error:

```
error[E0277]: can't compare `str` with `bool`
  --> src\main.rs:11:17
   |
11 | #[derive(Debug, Parser)]
   |                 ^^^^^^ no implementation for `str == bool`
   |
   = help: the trait `PartialEq<bool>` is not implemented for `str`
   = note: required because of the requirements on the impl of `PartialEq<&mut bool>` for `&str`
   = note: this error originates in the derive macro `Parser` (in Nightly builds, run wi
th -Z macro-backtrace for more info)

For more information about this error, try `rustc --explain E0277`.
error: could not compile `tmp-wfwrb0` due to previous error
```

### Expected Behaviour

This should compile as it does (minus the `s/Parser/Clap` rename) when `clap`'s version is specified as `=3.0.0-beta.4`.

### Additional Context

_No response_

### Debug Output

Same compile error output as in "Actual Behavior" above.

---

_Label `T: bug` added by @ErichDonGubler on 2021-10-23 16:36_

---

_Comment by @epage on 2021-10-23 16:54_

I think this is from a name collision in this function
```rust
        fn update_from_arg_matches<'b>(&mut self, arg_matches: &clap::ArgMatches) {
            if let Some((name, sub_arg_matches)) = arg_matches.subcommand() {
                match self {
                    Enum::Subcommand { ref mut name } if "subcommand" == name => {
                        let arg_matches = sub_arg_matches;
                        {
                            if arg_matches.is_present("name") {
                                *name = *name || arg_matches.is_present("name")
                            }
                        }
                    }
                    s => {
                        *s = <Self as clap::FromArgMatches>::from_arg_matches(arg_matches).unwrap();
                    }
                }
            }
        }
```

Looking at the history, it looks like this came from https://github.com/clap-rs/clap/pull/2751

---

_Referenced in [bellboy-dotfiles/bellboy#30](../../bellboy-dotfiles/bellboy/pulls/30.md) on 2021-10-23 17:10_

---

_Referenced in [clap-rs/clap#2935](../../clap-rs/clap/pulls/2935.md) on 2021-10-23 17:58_

---

_Closed by @bors[bot] on 2021-10-23 20:57_

---
