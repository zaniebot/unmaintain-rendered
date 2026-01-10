---
number: 4286
title: "Derive requires field types to impl `Clone`"
type: issue
state: open
author: djc
labels:
  - C-bug
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2022-09-29T08:22:23Z
updated_at: 2024-02-17T16:52:31Z
url: https://github.com/clap-rs/clap/issues/4286
synced_at: 2026-01-10T01:27:52Z
---

# Derive requires field types to impl `Clone`

---

_Issue opened by @djc on 2022-09-29 08:22_

Maintainer's notes
- For Sync+Sync, see #4347
---

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0 (a55dd71d5 2022-09-19)

### Clap Version

4.0.2

### Minimal reproducible code

```rust
#[derive(Debug, Parser)]
#[command(name = "topfew")]
struct Options {
    /// Fields to use as part of the line's key
    #[arg(long, short)]
    fields: FieldSpec,
}

#[derive(Debug)]
struct FieldSpec {
    indices: Vec<usize>,
}

impl FromStr for FieldSpec {
    type Err = Error;
    fn from_str(s: &str) -> Result<Self, Self::Err> {
        let mut indices = Vec::new();
        for f in s.split(',') {
            indices.push(usize::from_str(f)?);
        }
        Ok(FieldSpec { indices })
    }
}
```

### Steps to reproduce the bug with the above code

cargo check

### Actual Behaviour

Fails with an elaborate error pointing to many trait bounds with `_AutoValueParser<FieldSpec>`:

```rust
error[E0599]: the method `value_parser` exists for reference `&&&&&&_AutoValueParser<FieldSpec>`, but its trait bounds were not satisfied
    --> src/bin/tf.rs:29:5
     |
29   |     /// Fields to use as part of the line's key
     |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ method cannot be called on `&&&&&&_AutoValueParser<FieldSpec>` due to unsatisfied trait bounds
...
43   | struct FieldSpec {
     | ----------------
     | |
     | doesn't satisfy `FieldSpec: Clone`
     | doesn't satisfy `FieldSpec: From<&'s std::ffi::OsStr>`
     | doesn't satisfy `FieldSpec: From<&'s str>`
     | doesn't satisfy `FieldSpec: From<OsString>`
     | doesn't satisfy `FieldSpec: From<std::string::String>`
     | doesn't satisfy `FieldSpec: ValueEnum`
     | doesn't satisfy `FieldSpec: ValueParserFactory`
     |
    ::: /Users/djc/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.2/src/builder/value_parser.rs:2017:1
     |
2017 | pub struct _AutoValueParser<T>(std::marker::PhantomData<T>);
     | ------------------------------ doesn't satisfy `_: clap::builder::via_prelude::_ValueParserViaParse`
     |
     = note: the following trait bounds were not satisfied:
             `FieldSpec: ValueEnum`
             which is required by `&&&&&_AutoValueParser<FieldSpec>: clap::builder::via_prelude::_ValueParserViaValueEnum`
             `FieldSpec: Clone`
             which is required by `&&&&&_AutoValueParser<FieldSpec>: clap::builder::via_prelude::_ValueParserViaValueEnum`
             `FieldSpec: ValueParserFactory`
             which is required by `&&&&&&_AutoValueParser<FieldSpec>: clap::builder::via_prelude::_ValueParserViaFactory`
             `FieldSpec: From<OsString>`
             which is required by `&&&&_AutoValueParser<FieldSpec>: clap::builder::via_prelude::_ValueParserViaFromOsString`
             `FieldSpec: Clone`
             which is required by `&&&&_AutoValueParser<FieldSpec>: clap::builder::via_prelude::_ValueParserViaFromOsString`
             `FieldSpec: From<&'s std::ffi::OsStr>`
             which is required by `&&&_AutoValueParser<FieldSpec>: clap::builder::via_prelude::_ValueParserViaFromOsStr`
             `FieldSpec: Clone`
             which is required by `&&&_AutoValueParser<FieldSpec>: clap::builder::via_prelude::_ValueParserViaFromOsStr`
             `FieldSpec: From<std::string::String>`
             which is required by `&&_AutoValueParser<FieldSpec>: clap::builder::via_prelude::_ValueParserViaFromString`
             `FieldSpec: Clone`
             which is required by `&&_AutoValueParser<FieldSpec>: clap::builder::via_prelude::_ValueParserViaFromString`
             `FieldSpec: From<&'s str>`
             which is required by `&_AutoValueParser<FieldSpec>: clap::builder::via_prelude::_ValueParserViaFromStr`
             `FieldSpec: Clone`
             which is required by `&_AutoValueParser<FieldSpec>: clap::builder::via_prelude::_ValueParserViaFromStr`
             `FieldSpec: Clone`
             which is required by `_AutoValueParser<FieldSpec>: clap::builder::via_prelude::_ValueParserViaParse`
```

### Expected Behaviour

As in clap 3, this should just work.

### Additional Context

I find it very surprising that `From<&str>` and `From<String>` are supported, but `FromStr` or `TryFrom` impls are not.

### Debug Output

_No response_

---

_Label `C-bug` added by @djc on 2022-09-29 08:22_

---

_Comment by @epage on 2022-09-29 14:26_

Can you provide a complete reproduction case?  For example, I want to make sure I'm digging into this with the same error type you are using.

---

_Comment by @djc on 2022-09-29 16:12_

This compiles in the playground with clap 3:

```rust
use clap::Parser;
use anyhow::Error;

#[derive(Debug, Parser)]
#[clap(name = "topfew")]
struct Options {
    /// Fields to use as part of the line's key
    #[clap(long, short)]
    fields: FieldSpec,
}

#[derive(Debug)]
struct FieldSpec {
    indices: Vec<usize>,
}

impl std::str::FromStr for FieldSpec {
    type Err = Error;
    fn from_str(s: &str) -> Result<Self, Self::Err> {
        let mut indices = Vec::new();
        for f in s.split(',') {
            indices.push(usize::from_str(f)?);
        }
        Ok(FieldSpec { indices })
    }
}
```

---

_Comment by @epage on 2022-09-29 16:22_

My first step was to opt-in to clap v4's behavior with clap 3 by adding the `value_parser` attribute.  This required adding `Clone` to `FieldSpec`
```rust
#!/usr/bin/env -S rust-script

//! ```cargo
//! [dependencies]
//! clap = { version = "3", features = ["derive"] }
//! anyhow = "1"
//! ```

use anyhow::Error;
use clap::Parser;

#[derive(Debug, Parser)]
#[clap(name = "topfew")]
struct Options {
    /// Fields to use as part of the line's key
    #[clap(long, short, value_parser)]
    fields: FieldSpec,
}

#[derive(Debug, Clone)]
struct FieldSpec {
    indices: Vec<usize>,
}

impl std::str::FromStr for FieldSpec {
    type Err = Error;
    fn from_str(s: &str) -> Result<Self, Self::Err> {
        let mut indices = Vec::new();
        for f in s.split(',') {
            indices.push(usize::from_str(f)?);
        }
        Ok(FieldSpec { indices })
    }
}
```

---

_Comment by @epage on 2022-09-29 16:23_

And it turns out that was what was needed to get it working with clap v4
```rust
#!/usr/bin/env -S rust-script

//! ```cargo
//! [dependencies]
//! clap = { version = "4", features = ["derive"] }
//! anyhow = "1"
//! ```

use anyhow::Error;
use clap::Parser;

#[derive(Debug, Parser)]
#[clap(name = "topfew")]
struct Options {
    /// Fields to use as part of the line's key
    #[clap(long, short)]
    fields: FieldSpec,
}

#[derive(Debug, Clone)]
struct FieldSpec {
    indices: Vec<usize>,
}

impl std::str::FromStr for FieldSpec {
    type Err = Error;
    fn from_str(s: &str) -> Result<Self, Self::Err> {
        let mut indices = Vec::new();
        for f in s.split(',') {
            indices.push(usize::from_str(f)?);
        }
        Ok(FieldSpec { indices })
    }
}
```

---

_Comment by @epage on 2022-09-29 16:25_

While the listed error message wasn't too helpful in figuring out what was going on, earlier I got this error message
```
error[E0277]: the trait bound `FieldSpec: Clone` is not satisfied                                                                 --> clap-4286.rs:17:5                                                                                                           |
17  |     fields: FieldSpec,                                                                                                       |     ^^^^^^ the trait `Clone` is not implemented for `FieldSpec`                                                              |
note: required by a bound in `ArgMatches::remove_one`
   --> /home/epage/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.22/src/parser/matches/arg_matches.rs:309:32            |
309 |     pub fn remove_one<T: Any + Clone + Send + Sync + 'static>(&mut self, id: &str) -> Option<T> {                            |                                ^^^^^ required by this bound in `ArgMatches::remove_one`
help: consider annotating `FieldSpec` with `#[derive(Clone)]`
    |
21  | #[derive(Clone)]
    |
```

---

_Comment by @djc on 2022-09-29 16:27_

So it's just the `Clone` bound? Why is that necessary, though? It doesn't conceptually make sense to me.

---

_Comment by @epage on 2022-09-29 16:34_

That is an artifact of the derive API being built on top of the builder API.  This is happening on two layers
- `ArgMatches` impls `Clone`
- As an implementation detail, for now we wrap the value inside `ArgMatches` inside an `Arc` but to move it out, we still need a `Clone` in case there are multiple references to it.

---

_Renamed from "Regression: clap 4 no longer supports FromStr for arguments" to "Derive requires field types to impl `Clone`" by @epage on 2022-10-04 16:23_

---

_Label `A-derive` added by @epage on 2022-10-04 16:23_

---

_Label `S-waiting-on-design` added by @epage on 2022-10-04 16:23_

---

_Referenced in [clap-rs/clap#4347](../../clap-rs/clap/issues/4347.md) on 2022-10-04 16:24_

---

_Comment by @epage on 2022-10-10 13:44_

@nbari please be sure to include full reproduction cases and compiler errors so we can make sure we are helping you with your actual problem and not a bad guess as to what your problem is.

As is, I'm not too sure why you posted on this issue and removed `std::str::FromStr`.  The fact that you pointed out the `version` line though makes me think you need to add the `string` feature flag in cargo (`cargo add clap -F string`).  This is what enables clap to work with runtime values rather than just `&'static str`.

If you are curious about the background on why this is a feature flag, https://epage.github.io/blog/2022/09/clap4/#removing-lifetimes goes into some details.

I can see us having an Issue or Discussion on how to make this more obvious.

---

_Referenced in [bazelbuild/rules_rust#1591](../../bazelbuild/rules_rust/pulls/1591.md) on 2022-10-12 03:48_

---

_Comment by @dayo777 on 2023-06-26 15:42_

I just came across this error on Clap V4.3.8, rust V1.67.1;


---

_Comment by @linde12 on 2023-09-05 16:55_

I'm struggling with a similar error. I'm unable to find any documentation how to do custom parsers in general and error messages are terrible :-/ maybe anyone knows what to do?

https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=f53995eebe24313652f70a74fb9f1779
```rust
use clap::Parser;
use std::str::FromStr;
use std::fmt;

enum MyError {
    Exploded
}

impl fmt::Display for MyError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "boom!")
    }
}

#[derive(Clone)]
struct Foo;

impl FromStr for Foo {
    type Err = MyError;

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        Ok(Foo)
    }
}

#[derive(Parser)]
struct Cli {
    #[arg(short, long)]
    foo: Foo,
}

fn main() {
    let _cli = Cli::parse_from(vec!["program", "--foo=10z"]);
}
```

Error messages is kind of bad but this caught my eye:

```
5    | enum MyError {
     | ------------ doesn't satisfy `_: Into<Box<dyn Error + Send + Sync>>
```

Do i have to implement `Into<Box>`? If so, why and if not then what? Apologies if i'm missing something obvious - still pretty new to Rust in general :-)

---

_Comment by @marienz on 2023-09-10 06:44_

> Do i have to implement `Into<Box>`? If so, why and if not then what?

The important part of that error message is `dyn Error`. Using https://github.com/dtolnay/thiserror to derive `Error` for `MyError` makes it compile, see https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=cef9827b74c2b19030686599332d9876. Clap cannot accept just any error type, as it needs to be able to format the error (for inclusion in an error message).

(I don't think your problem is related to this issue, even though part of the error message is the same. The error message is a bit unwieldy because there are several traits available for parsing arguments of your custom type, and the compiler does not know which one you intended to use, so it ends up listing all of them along with the reason it could not use them.)

---

_Referenced in [awslabs/tough#678](../../awslabs/tough/pulls/678.md) on 2023-09-27 17:54_

---

_Comment by @SteveLauC on 2024-02-17 04:52_

> I find it very surprising that From<&str> and From<String> are supported, but FromStr or TryFrom impls are not.

This behavior, is still present with the latest version of clap 4.5.1:

```rs
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

```sh
$ cargo --version
cargo 1.76.0 (c84b36747 2024-01-18)

$ cargo b
   Compiling rust v0.1.0 (/home/steve/Documents/workspace/rust)
error[E0599]: the method `value_parser` exists for reference `&&&&&&_AutoValueParser<Foo>`, but its trait bounds were not satisfied
    --> src/main.rs:6:5
     |
6    |     #[arg(short, long)]
     |     ^ method cannot be called on `&&&&&&_AutoValueParser<Foo>` due to unsatisfied trait bounds
...
11   | struct Foo;
     | ----------
     | |
     | doesn't satisfy `Foo: From<&'s std::ffi::OsStr>`
     | doesn't satisfy `Foo: From<&'s str>`
     | doesn't satisfy `Foo: From<OsString>`
     | doesn't satisfy `Foo: From<std::string::String>`
     | doesn't satisfy `Foo: ValueEnum`
     | doesn't satisfy `Foo: ValueParserFactory`
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


I have to `impl From<String> for Foo` to make it compile:

```rs
impl From<String> for Foo {
    fn from(value: String) -> Self {
        todo!()
    }
}
```

```sh
$ cargo b
   Compiling rust v0.1.0 (/home/steve/Documents/workspace/rust)
warning: unused variable: `s`
  --> src/main.rs:15:17
   |
15 |     fn from_str(s: &str) -> Result<Self, Self::Err> {
   |                 ^ help: if this is intentional, prefix it with an underscore: `_s`
   |
   = note: `#[warn(unused_variables)]` on by default

warning: unused variable: `value`
  --> src/main.rs:21:13
   |
21 |     fn from(value: String) -> Self {
   |             ^^^^^ help: if this is intentional, prefix it with an underscore: `_value`

warning: `rust` (bin "rust") generated 2 warnings (run `cargo fix --bin "rust"` to apply 2 suggestions)
    Finished dev [unoptimized + debuginfo] target(s) in 0.26s

$ echo $?
0
```


---

_Comment by @SteveLauC on 2024-02-17 06:45_

For the issue described in my last comment, the error message will go away if you give `<Foo as FromStr>::Err` a type other than `()`, I found this solution in [this comment](https://github.com/clap-rs/clap/issues/4994#issuecomment-1658453115).

Update: I filed an issue for it: #5360

---

_Referenced in [clap-rs/clap#5360](../../clap-rs/clap/issues/5360.md) on 2024-02-17 06:59_

---
