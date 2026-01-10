---
number: 2975
title: "Compiler error with `ArgEnum` and multi-line doc comments on variants"
type: issue
state: closed
author: graelo
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2021-10-31T08:29:03Z
updated_at: 2021-11-03T23:42:26Z
url: https://github.com/clap-rs/clap/issues/2975
synced_at: 2026-01-10T01:27:30Z
---

# Compiler error with `ArgEnum` and multi-line doc comments on variants

---

_Issue opened by @graelo on 2021-10-31 08:29_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.56.0 (09c42c458 2021-10-18)

### Clap Version

3.0.0-beta.5

### Minimal reproducible code

Content of `Cargo.toml`:

```rust
[package]
name = "repro"
version = "0.1.0"
edition = "2018"

[dependencies]
clap = { version = "3.0.0-beta.5", features = ["derive"] }
```

Content of `src/main.rs`:

```rust
use clap::{ArgEnum, Parser};

/// Some random enum
#[derive(Debug, Clone, ArgEnum)]
pub enum Field {
    /// Variant1 doc
    ///
    /// The long_about doc that is not accepted anymore.
    VariantOne,

    /// Variant2 doc
    VariantTwo,
}

/// Main configuration, parsed from command line.
#[derive(Parser, Debug)]
pub struct Config {
    /// Field documentation
    #[clap(arg_enum, rename_all = "kebab-case", default_value = "variant-one")]
    pub field: Field,
}

fn main() {
    let _ = Config::parse();
}
```


### Steps to reproduce the bug with the above code

```sh
cargo build
```

### Actual Behaviour

The compiler returns the following error

```text
error[E0599]: no method named `long_about` found for struct `ArgValue` in the current scope
 --> src/main.rs:4:24
  |
4 | #[derive(Debug, Clone, ArgEnum)]
  |                        ^^^^^^^ method not found in `ArgValue<'_>`
  |
  = note: this error originates in the derive macro `ArgEnum` (in Nightly builds, run with -Z macro-backtrace for more info)

For more information about this error, try `rustc --explain E0599`.
error: could not compile `repro` due to previous error
```

### Expected Behaviour

I _think_ that `long_about` docs should not generate compiler errors, as it was the case before `beta.5`.

### Additional Context

While migrating from `beta.2` and `beta.4` to `beta.5` I encountered misc issues I discussed with @epage: https://github.com/clap-rs/clap/discussions/2932

### Debug Output

Exact same error as above.

---

_Label `T: bug` added by @graelo on 2021-10-31 08:29_

---

_Label `C: derive macros` added by @epage on 2021-11-01 13:57_

---

_Renamed from "`3.0.0-beta.5` does not allow `long_about` docs for enum variants." to "Compiler error with `ArgEnum` and multi-line doc comments on variants" by @epage on 2021-11-01 13:59_

---

_Comment by @epage on 2021-11-01 14:00_

We don't do a good job of distinguishing between Args, Subcommands, ArgEnums, etc when generating method calls

---

_Comment by @marjakm on 2021-11-03 20:54_

I think this should be fixed in master branch by my pull request:
https://github.com/clap-rs/clap/pull/2916

---

_Comment by @epage on 2021-11-03 22:44_

You're right, I forgot about that having gone in.

---

_Closed by @epage on 2021-11-03 22:44_

---

_Comment by @graelo on 2021-11-03 23:42_

Fantastic, thanks!

---
