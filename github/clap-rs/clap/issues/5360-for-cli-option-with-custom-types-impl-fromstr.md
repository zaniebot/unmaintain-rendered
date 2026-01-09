---
number: 5360
title: "For cli option with custom types, `impl FromStr` with `FromStr::Err` set to `()` won't work"
type: issue
state: open
author: SteveLauC
labels:
  - C-bug
assignees: []
created_at: 2024-02-17T06:59:31Z
updated_at: 2024-02-18T00:43:22Z
url: https://github.com/clap-rs/clap/issues/5360
synced_at: 2026-01-07T13:12:20-06:00
---

# For cli option with custom types, `impl FromStr` with `FromStr::Err` set to `()` won't work

---

_Issue opened by @SteveLauC on 2024-02-17 06:59_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.76.0 (07dca489a 2024-02-04)

### Clap Version

4.5.1

### Minimal reproducible code

```rust
use std::str::FromStr;
use clap::Parser;

#[derive(Parser)]
struct Cli {
    #[arg(short, long)]
    foo: Foo,
}

#[derive(Clone)]
struct Foo;

impl FromStr for Foo {
    type Err = ();
    fn from_str(s: &str) -> Result<Self, Self::Err> {
        todo!()
    }
}

fn main() {}
```


### Steps to reproduce the bug with the above code

With the above code, run `cargo build`

### Actual Behaviour

```
   Compiling rust v0.1.0 (/home/steve/Documents/workspace/rust)
error[E0599]: the method `value_parser` exists for reference `&&&&&&_AutoValueParser<Foo>`, but its trait bounds were not satisfied
    --> src/main.rs:17:5
     |
5    | struct Foo;
     | ----------
     | |
     | doesn't satisfy `Foo: From<&'s std::ffi::OsStr>`
     | doesn't satisfy `Foo: From<&'s str>`
     | doesn't satisfy `Foo: From<OsString>`
     | doesn't satisfy `Foo: From<std::string::String>`
     | doesn't satisfy `Foo: ValueEnum`
     | doesn't satisfy `Foo: ValueParserFactory`
...
17   |     #[arg(short, long)]
     |     ^ method cannot be called on `&&&&&&_AutoValueParser<Foo>` due to unsatisfied trait bounds
     |
    ::: /home/steve/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.1/src/builder/value_parser.rs:2462:1
     |
2462 | pub struct _AutoValueParser<T>(std::marker::PhantomData<T>);
     | ------------------------------ doesn't satisfy `_: _ValueParserViaParse`
     |
     = note: the following trait bounds were not satisfied:
             `Foo: ValueEnum`
             which is required by `&&&&&_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaValueEnum`
             `Foo: ValueParserFactory`
             which is required by `&&&&&&_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaFactory`
             `Foo: From<OsString>`
             which is required by `&&&&_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaFromOsString`
             `Foo: From<&'s std::ffi::OsStr>`
             which is required by `&&&_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaFromOsStr`
             `Foo: From<std::string::String>`
             which is required by `&&_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaFromString`
             `Foo: From<&'s str>`
             which is required by `&_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaFromStr`
             `(): Into<Box<(dyn std::error::Error + Send + Sync + 'static)>>`
             which is required by `_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaParse`
note: the traits `ValueEnum`, `ValueParserFactory`,  and `From` must be implemented
    --> /home/steve/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.1/src/builder/value_parser.rs:2312:1
     |
2312 | pub trait ValueParserFactory {
     | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
     |
    ::: /home/steve/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.1/src/derive.rs:264:1
     |
264  | pub trait ValueEnum: Sized + Clone {
     | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
     |
    ::: /home/steve/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/convert/mod.rs:579:1
     |
579  | pub trait From<T>: Sized {
     | ^^^^^^^^^^^^^^^^^^^^^^^^
     = note: this error originates in the macro `clap::value_parser` (in Nightly builds, run with -Z macro-backtrace for more info)

For more information about this error, try `rustc --explain E0599`.
error: could not compile `rust` (bin "rust") due to 2 previous errors
```

### Expected Behaviour

Build without issues

### Additional Context

Related issues:

1. #4286
   
    From this issue, I found you can fix it by `impl From<String> for Foo`.

```rs
use std::str::FromStr;
use clap::Parser;

#[derive(Clone)]
struct Foo;

impl FromStr for Foo {
    type Err = ();
    fn from_str(s: &str) -> Result<Self, Self::Err> {
        todo!()
    }
}

impl From<String> for Foo {
    fn from(value: String) -> Self {
        todo!()
    }
}

#[derive(Parser)]
struct Cli {
    #[arg(short, long)]
    foo: Foo,
}

fn main() {
    let cli = Cli::parse();
}
```
```sh
$ cargo b
$ echo $?
0
```

2. #4994 

    From this one, I found that you can also fix it by changing the `()` type to other types as described in [this comment](https://github.com/clap-rs/clap/issues/4994#issuecomment-1658453115):

```rs
use std::str::FromStr;
use clap::Parser;

#[derive(Clone)]
struct Foo;

impl FromStr for Foo {
    type Err = String;
    fn from_str(s: &str) -> Result<Self, Self::Err> {
        todo!()
    }
}


#[derive(Parser)]
struct Cli {
    #[arg(short, long)]
    foo: Foo,
}

fn main() {
    let cli = Cli::parse();
}
```

```sh
$ cargo b 
$ echo $?
0
```

### Debug Output

```
error[E0599]: the method `value_parser` exists for reference `&&&&&&_AutoValueParser<Foo>`, but its trait bounds were not satisfied
    --> src/main.rs:17:5
     |
5    | struct Foo;
     | ----------
     | |
     | doesn't satisfy `Foo: From<&'s std::ffi::OsStr>`
     | doesn't satisfy `Foo: From<&'s str>`
     | doesn't satisfy `Foo: From<OsString>`
     | doesn't satisfy `Foo: From<std::string::String>`
     | doesn't satisfy `Foo: ValueEnum`
     | doesn't satisfy `Foo: ValueParserFactory`
...
17   |     #[arg(short, long)]
     |     ^ method cannot be called on `&&&&&&_AutoValueParser<Foo>` due to unsatisfied trait bounds
     |
    ::: /home/steve/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.1/src/builder/value_parser.rs:2462:1
     |
2462 | pub struct _AutoValueParser<T>(std::marker::PhantomData<T>);
     | ------------------------------ doesn't satisfy `_: _ValueParserViaParse`
     |
     = note: the following trait bounds were not satisfied:
             `Foo: ValueEnum`
             which is required by `&&&&&_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaValueEnum`
             `Foo: ValueParserFactory`
             which is required by `&&&&&&_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaFactory`
             `Foo: From<OsString>`
             which is required by `&&&&_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaFromOsString`
             `Foo: From<&'s std::ffi::OsStr>`
             which is required by `&&&_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaFromOsStr`
             `Foo: From<std::string::String>`
             which is required by `&&_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaFromString`
             `Foo: From<&'s str>`
             which is required by `&_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaFromStr`
             `(): Into<Box<(dyn std::error::Error + Send + Sync + 'static)>>`
             which is required by `_AutoValueParser<Foo>: clap::builder::via_prelude::_ValueParserViaParse`
note: the traits `ValueEnum`, `ValueParserFactory`,  and `From` must be implemented
    --> /home/steve/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.1/src/builder/value_parser.rs:2312:1
     |
2312 | pub trait ValueParserFactory {
     | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
     |
    ::: /home/steve/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.1/src/derive.rs:264:1
     |
264  | pub trait ValueEnum: Sized + Clone {
     | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
     |
    ::: /home/steve/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/convert/mod.rs:579:1
     |
579  | pub trait From<T>: Sized {
     | ^^^^^^^^^^^^^^^^^^^^^^^^
     = note: this error originates in the macro `clap::value_parser` (in Nightly builds, run with -Z macro-backtrace for more info)

For more information about this error, try `rustc --explain E0599`.
error: could not compile `rust` (bin "rust") due to 2 previous errors
```

---

_Label `C-bug` added by @SteveLauC on 2024-02-17 06:59_

---

_Referenced in [clap-rs/clap#4994](../../clap-rs/clap/issues/4994.md) on 2024-02-17 07:01_

---

_Referenced in [clap-rs/clap#4286](../../clap-rs/clap/issues/4286.md) on 2024-02-17 07:01_

---

_Comment by @epage on 2024-02-17 16:49_

If I'm understanding correctly, you are saying that `Foo::from_str` can fail. In that case, we need to be able to render a message but we don't know how to render a message for `()`. It seems like an error is the correct thing to do.

---

_Comment by @SteveLauC on 2024-02-18 00:41_

> In that case, we need to be able to render a message but we don't know how to render a message for (). It seems like an error is the correct thing to do.

This is understandable. I am wondering can we:

1. Give a better error message stating that we don't know how to render the error message for the unit type.
2. [Document it](https://docs.rs/clap/latest/clap/macro.value_parser.html)

Either will make the case better



---

_Comment by @epage on 2024-02-18 00:43_

We can't improve the error message.

As for documenting it, the challenge is finding the right places where someone is expected to look and won't add to a wall of text that will make it so no one will read any of it.

---
