```yaml
number: 4994
title: "Derive: specifying invalid struct field type fails with Epic error message"
type: issue
state: closed
author: LunarLambda
labels:
  - C-bug
assignees: []
created_at: 2023-07-05T11:57:00Z
updated_at: 2024-02-17T07:01:13Z
url: https://github.com/clap-rs/clap/issues/4994
synced_at: 2026-01-12T16:14:16Z
```

# Derive: specifying invalid struct field type fails with Epic error message

---

_@LunarLambda_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.70

### Clap Version

4.3.10

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser)]
struct Cli {
    #[arg(short = 'C')]
    foo: Option<Box<std::path::Path>>
}

fn main() {
    let cli = Cli::parse();
}
```


### Steps to reproduce the bug with the above code

cargo check

### Actual Behaviour

The user is presented with a truly threatening error message:

```
error[E0599]: the method `value_parser` exists for reference `&&&&&&_AutoValueParser<Box<Path>>`, but its trait bounds were not satisfied
    --> src/main.rs:5:5
     |
5    |       #[arg(short = 'C')]
     |       ^ method cannot be called on `&&&&&&_AutoValueParser<Box<Path>>` due to unsatisfied trait bounds
     |
    ::: /home/luna/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.3.10/src/builder/value_parser.rs:2221:1
     |
2221 |   pub struct _AutoValueParser<T>(std::marker::PhantomData<T>);
     |   ------------------------------ doesn't satisfy `_: _ValueParserViaParse`
     |
    ::: /home/luna/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/boxed.rs:195:1
     |
195  | / pub struct Box<
196  | |     T: ?Sized,
197  | |     #[unstable(feature = "allocator_api", issue = "32838")] A: Allocator = Global,
198  | | >(Unique<T>, A);
     | | -
     | | |
     | | doesn't satisfy `Box<Path>: From<&'s std::ffi::OsStr>`
     | | doesn't satisfy `Box<Path>: From<&'s str>`
     | | doesn't satisfy `Box<Path>: From<OsString>`
     | | doesn't satisfy `Box<Path>: From<std::string::String>`
     | | doesn't satisfy `Box<Path>: FromStr`
     | |_doesn't satisfy `Box<Path>: ValueEnum`
     |   doesn't satisfy `Box<Path>: ValueParserFactory`
     |
     = note: the following trait bounds were not satisfied:
             `Box<Path>: ValueEnum`
             which is required by `&&&&&_AutoValueParser<Box<Path>>: clap::builder::via_prelude::_ValueParserViaValueEnum`
             `Box<Path>: ValueParserFactory`
             which is required by `&&&&&&_AutoValueParser<Box<Path>>: clap::builder::via_prelude::_ValueParserViaFactory`
             `Box<Path>: From<OsString>`
             which is required by `&&&&_AutoValueParser<Box<Path>>: clap::builder::via_prelude::_ValueParserViaFromOsString`
             `Box<Path>: From<&'s std::ffi::OsStr>`
             which is required by `&&&_AutoValueParser<Box<Path>>: clap::builder::via_prelude::_ValueParserViaFromOsStr`
             `Box<Path>: From<std::string::String>`
             which is required by `&&_AutoValueParser<Box<Path>>: clap::builder::via_prelude::_ValueParserViaFromString`
             `Box<Path>: From<&'s str>`
             which is required by `&_AutoValueParser<Box<Path>>: clap::builder::via_prelude::_ValueParserViaFromStr`
             `Box<Path>: FromStr`
             which is required by `_AutoValueParser<Box<Path>>: clap::builder::via_prelude::_ValueParserViaParse`
     = note: this error originates in the macro `clap::value_parser` (in Nightly builds, run with -Z macro-backtrace for more info)
```

### Expected Behaviour

The derive macro should produce a human-readable error when it detects an invalid type being used, rather than produce a horrific type error after expansion,

Additionally, it would be nice if parsing into smart pointers was supported. Using a `PathBuf` would be wasteful in my case, as I don't intend on modifying the path, and would like to encode this directly into my type. Using Rc or Arc would also allow cheaply cloning the struct to pass it to other code. Serde allows this afaik.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @LunarLambda on 2023-07-05 11:57_

---

_Comment by @LunarLambda on 2023-07-05 12:12_

Worked around it like so (which is a little ugly, but it works)

```rs
use clap::builder::{PathBufValueParser, TypedValueParser};

fn box_path() -> impl TypedValueParser<Value = Box<Path>> {
    PathBufValueParser::new().map(|p| p.into())
}

#[derive(Parser)] 
struct Cli {
    #[arg(short = 'C', value_parser = box_path())]
    foo: Option<Box<Path>>
}
```

---

_Comment by @epage on 2023-07-05 14:17_

> The derive macro should produce a human-readable error when it detects an invalid type being used, rather than produce a horrific type error after expansion,

The derive macro can only see the textual name for a type (ie `PathBuf` and `std::path::PathBuf` are different).  We are delegating to rust code to map types to value parsers so we don't have to worry about how a type is named and to workaround the lack of specialization in the language (having a single trait with a blanket impl plus some hand impls)

A subset of that error actually provides some really good hints
```
     | | doesn't satisfy `Box<Path>: From<&'s std::ffi::OsStr>`
     | | doesn't satisfy `Box<Path>: From<&'s str>`
     | | doesn't satisfy `Box<Path>: From<OsString>`
     | | doesn't satisfy `Box<Path>: From<std::string::String>`
     | | doesn't satisfy `Box<Path>: FromStr`
     | |_doesn't satisfy `Box<Path>: ValueEnum`
     |   doesn't satisfy `Box<Path>: ValueParserFactory`
```
though it can't also suggest your workaround.

---

_Referenced in [clap-rs/clap#4995](../../clap-rs/clap/pulls/4995.md) on 2023-07-05 15:13_

---

_Comment by @epage on 2023-07-05 15:13_

#4995 adds support for some serde types

---

_Comment by @epage on 2023-07-07 19:09_

As part of the message is intentional and our options are limited for resolving this and with us adding support for more types, I'm not seeing what more can be done for this and am closing this.

If this needs further discussion, feel free to say something!

---

_Closed by @epage on 2023-07-07 19:09_

---

_Comment by @Kobzol on 2023-07-31 14:12_

I also hit this error message when implementing `FromStr` with an `Err` type of `()` (https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=99878a7c31bf79048c92a77c06096969). It was quite confusing to me, until I realized that the problem goes away when `Err` gets set to `String`.

---

_Referenced in [clap-rs/clap#4286](../../clap-rs/clap/issues/4286.md) on 2024-02-17 06:45_

---

_Referenced in [clap-rs/clap#5360](../../clap-rs/clap/issues/5360.md) on 2024-02-17 06:59_

---

_Comment by @SteveLauC on 2024-02-17 07:01_

Hi @Kobzol, thanks for that comment (and workaround), I filed an issue for it: #5360

---
