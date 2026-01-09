---
number: 3335
title: "`name` implies a user-facing meaning, causing confusion"
type: issue
state: closed
author: pwinckles
labels:
  - C-bug
  - M-breaking-change
  - A-builder
  - S-experimental
assignees: []
created_at: 2022-01-24T16:41:58Z
updated_at: 2022-02-11T20:28:41Z
url: https://github.com/clap-rs/clap/issues/3335
synced_at: 2026-01-07T13:12:19-06:00
---

# `name` implies a user-facing meaning, causing confusion

---

_Issue opened by @pwinckles on 2022-01-24 16:41_

Maintainer's notes:
- The root cause was [confusion over `name` vs `value_name` and impact on the rest of the API](https://github.com/clap-rs/clap/issues/3335#issuecomment-1020308017)
- We should look into renaming `Arg::name` to `Arg::id`
- We should also resolve https://github.com/clap-rs/clap/issues/2475 which is possible because we (should be) setting `value_name` on people's behalf.
---
### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.58.1 (db9d1b20b 2022-01-20)

### Clap Version

3.0.10

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Debug, Parser)]
pub struct Example {
    #[clap(short = 'S', long, conflicts_with = "version")]
    pub staged: bool,

    #[clap(name = "OBJ_ID")]
    pub object_id: String,

    #[clap(name = "VERSION")]
    pub version: Option<String>,
}

fn main() {
    println!("{:?}", Example::parse());
}
```


### Steps to reproduce the bug with the above code

```shell
cargo run -- asdf
```


### Actual Behaviour

Panics:

```
thread 'main' panicked at 'App clap-example: Argument or group 'version' specified in 'conflicts_with*' for 'staged' does not exist', /home/pwinckles/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.10/src/build/app/debug_asserts.rs:171:13
```

### Expected Behaviour

It should not panic. I would expect Clap to enforce that the `staged` flag cannot be set if a `version` positional arg is provided, and otherwise allow the flag to be present or not.

### Additional Context

`conflicts_with` works as expected when conflicting with another flag. The problem seems to be when conflicting with an optional positional arg.

### Debug Output

```
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:clap-example
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Removing generated version
[            clap::build::app] 	App::_propagate_global_args:clap-example
[            clap::build::app] 	App::_derive_display_order:clap-example
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:staged
```

---

_Label `C-bug` added by @pwinckles on 2022-01-24 16:41_

---

_Renamed from "`conflicts_with` on a flag does not work when conflicting with a positional arg" to "`conflicts_with` on a flag panics when conflicting with a positional arg" by @pwinckles on 2022-01-24 16:46_

---

_Comment by @epage on 2022-01-24 16:49_

```rust
use clap::Parser;

#[derive(Debug, Parser)]
pub struct Example {
    #[clap(short = 'S', long, conflicts_with = "version")]
    pub staged: bool,

    #[clap(name = "OBJ_ID")]
    pub object_id: String,

    #[clap(name = "VERSION")]
    pub version: Option<String>,
}

fn main() {
    println!("{:?}", Example::parse());
}
```
should be
```rust
use clap::Parser;

#[derive(Debug, Parser)]
pub struct Example {
    #[clap(short = 'S', long, conflicts_with = "VERSION")]
    pub staged: bool,

    #[clap(name = "OBJ_ID")]
    pub object_id: String,

    #[clap(name = "VERSION")]
    pub version: Option<String>,
}

fn main() {
    println!("{:?}", Example::parse());
}
```

With `name`, you overrode the ID for referencing the version argument within clap, so you need to update references to that ID as well.

Most likely what you want is `value_name` instead of `name` (we do default `value_name` based on `name`).
```rust
use clap::Parser;

#[derive(Debug, Parser)]
pub struct Example {
    #[clap(short = 'S', long, conflicts_with = "version")]
    pub staged: bool,

    #[clap(value_name = "OBJ_ID")]
    pub object_id: String,

    #[clap(value_name = "VERSION")]
    pub version: Option<String>,
}

fn main() {
    println!("{:?}", Example::parse());
}
```


This kind of confusion is why I want to rename it from `name` to `id`.

---

_Comment by @pwinckles on 2022-01-24 16:52_

:blush:

Thank you! You are correct that I was confusing `name` with `value_name`. Sorry for the trouble.

---

_Renamed from "`conflicts_with` on a flag panics when conflicting with a positional arg" to "`name` implies a user-facing meaning, causing confusion" by @epage on 2022-01-24 16:57_

---

_Label `A-builder` added by @epage on 2022-01-24 16:57_

---

_Label `S-experimental` added by @epage on 2022-01-24 16:57_

---

_Comment by @epage on 2022-01-24 17:00_

Keeping this open as a use case for why we should explore changing `Arg::name`.

---

_Comment by @pwinckles on 2022-01-24 17:14_

For what it's worth, I experienced the issue because I migrated from clap 2 + structopt, and structopt includes an example like the following in its docs:

```
    /// File name: only required when `out-type` is set to `file`
    #[structopt(name = "FILE", required_if("out-type", "file"))]
    file_name: Option<String>,
```

I looked at the docs for clap 3, and I do think it's clear what `name` does, though I agree that it isn't intuitive without reading the docs.

---

_Label `M-breaking-change` added by @epage on 2022-01-24 17:28_

---

_Added to milestone `4.0` by @epage on 2022-01-24 17:28_

---

_Referenced in [clap-rs/clap#2475](../../clap-rs/clap/issues/2475.md) on 2022-02-11 20:13_

---

_Referenced in [clap-rs/clap#3453](../../clap-rs/clap/pulls/3453.md) on 2022-02-11 20:13_

---

_Closed by @epage on 2022-02-11 20:28_

---
