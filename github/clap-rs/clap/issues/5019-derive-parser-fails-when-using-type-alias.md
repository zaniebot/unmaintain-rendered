---
number: 5019
title: "`derive(Parser)` fails when using type alias"
type: issue
state: closed
author: mamekoro
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2023-07-19T11:51:14Z
updated_at: 2023-07-19T13:50:25Z
url: https://github.com/clap-rs/clap/issues/5019
synced_at: 2026-01-07T13:12:20-06:00
---

# `derive(Parser)` fails when using type alias

---

_Issue opened by @mamekoro on 2023-07-19 11:51_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.71.0 (8ede3aae2 2023-07-12)

### Clap Version

4.3.16

### Minimal reproducible code

```rust
/*
[dependencies]
clap = { version = "4.3.16", features = ["derive"] }
*/

use clap::Parser;

pub type MountOpt = Vec<String>;

#[derive(Clone, Debug, Parser)]
pub struct Args {
    #[arg(long, value_name = "mount")]
    pub mount: Option<MountOpt>,
}

fn main() {
    println!("{:?}", Args::parse());
}
```

### Steps to reproduce the bug with the above code

`cargo build`

### Actual Behaviour

```text
error[E0599]: the method `value_parser` exists for reference `&&&&&&_AutoValueParser<Vec<String>>`, but its trait bounds were not satisfied
    --> src/main.rs:12:5
     |
12   |     #[arg(long, value_name = "mount")]
     |     ^ method cannot be called on `&&&&&&_AutoValueParser<Vec<String>>` due to unsatisfied trait bounds
     |
    ::: <HOME>/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.3.16/src/builder/value_parser.rs:2274:1
     |
2274 | pub struct _AutoValueParser<T>(std::marker::PhantomData<T>);
     | ------------------------------ doesn't satisfy `_: _ValueParserViaParse`
     |
    ::: <HOME>/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/vec/mod.rs:396:1
     |
396  | pub struct Vec<T, #[unstable(feature = "allocator_api", issue = "32838")] A: Allocator = Global...
     | ------------------------------------------------------------------------------------------------
     | |
     | doesn't satisfy `Vec<std::string::String>: From<&'s str>`
     | doesn't satisfy `Vec<std::string::String>: From<OsString>`
     | doesn't satisfy `Vec<std::string::String>: FromStr`
     | doesn't satisfy `Vec<std::string::String>: ValueEnum`
     | doesn't satisfy `Vec<std::string::String>: ValueParserFactory`
     | doesn't satisfy `_: From<&OsStr>`
     | doesn't satisfy `_: From<String>`
     |
     = note: the following trait bounds were not satisfied:
             `Vec<std::string::String>: ValueEnum`
             which is required by `&&&&&_AutoValueParser<Vec<std::string::String>>: clap::builder::via_prelude::_ValueParserViaValueEnum`
             `Vec<std::string::String>: ValueParserFactory`
             which is required by `&&&&&&_AutoValueParser<Vec<std::string::String>>: clap::builder::via_prelude::_ValueParserViaFactory`
             `Vec<std::string::String>: From<OsString>`
             which is required by `&&&&_AutoValueParser<Vec<std::string::String>>: clap::builder::via_prelude::_ValueParserViaFromOsString`
             `Vec<std::string::String>: From<&'s std::ffi::OsStr>`
             which is required by `&&&_AutoValueParser<Vec<std::string::String>>: clap::builder::via_prelude::_ValueParserViaFromOsStr`
             `Vec<std::string::String>: From<std::string::String>`
             which is required by `&&_AutoValueParser<Vec<std::string::String>>: clap::builder::via_prelude::_ValueParserViaFromString`
             `Vec<std::string::String>: From<&'s str>`
             which is required by `&_AutoValueParser<Vec<std::string::String>>: clap::builder::via_prelude::_ValueParserViaFromStr`
             `Vec<std::string::String>: FromStr`
             which is required by `_AutoValueParser<Vec<std::string::String>>: clap::builder::via_prelude::_ValueParserViaParse`
     = note: this error originates in the macro `clap::value_parser` (in Nightly builds, run with -Z macro-backtrace for more info)

For more information about this error, try `rustc --explain E0599`.
error: could not compile `test-clap` (bin "test-clap") due to 2 previous errors
```

### Expected Behaviour

Compilation should success.

### Additional Context

I was writing a program to communicate with a Docker server, using Docker CLI (written in Go) as a reference.
I decided to declare type aliases to make the types in my code and Docker CLI similar, and I faced this problem.

### Debug Output

The same as "Actual Behaviour" above. (checked with the `diff` command.)

---

_Label `C-bug` added by @mamekoro on 2023-07-19 11:51_

---

_Comment by @epage on 2023-07-19 13:50_

This is actually expected behavior and is a [documented](https://docs.rs/clap/latest/clap/_derive/index.html#arg-types) way to disable the derive from inferring behavior from the type.

Closing in favor of #4626 which is discussing how to improve this

---

_Closed by @epage on 2023-07-19 13:50_

---

_Label `A-derive` added by @epage on 2023-07-19 13:50_

---
