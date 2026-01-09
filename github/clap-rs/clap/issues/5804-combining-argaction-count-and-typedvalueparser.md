---
number: 5804
title: "Combining `ArgAction::Count` and `TypedValueParser::map` doesn't seem to work."
type: issue
state: open
author: mkrasnitski
labels:
  - C-bug
assignees: []
created_at: 2024-11-02T07:45:46Z
updated_at: 2024-11-04T18:01:33Z
url: https://github.com/clap-rs/clap/issues/5804
synced_at: 2026-01-07T13:12:20-06:00
---

# Combining `ArgAction::Count` and `TypedValueParser::map` doesn't seem to work.

---

_Issue opened by @mkrasnitski on 2024-11-02 07:45_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.82.0 (f6e511eec 2024-10-15)

### Clap Version

4.5.19

### Minimal reproducible code

```rust
use clap::{ArgAction, Parser};
use clap::builder::TypedValueParser;

#[derive(Parser)]
struct Config {
    #[clap(
        short = 'c',
        action = ArgAction::Count, 
        value_parser = clap::value_parser!(u8).map(CleanLevel::from_val)
    )]
    clean: Option<CleanLevel>
}

#[derive(Clone)]
enum CleanLevel {
    Untracked,
    All,
}

impl CleanLevel {
    fn from_val(val: u8) -> Option<Self> {
        match val {
            0 => None,
            1 => Some(CleanLevel::Untracked),
            2.. => Some(CleanLevel::All),
        }
    }
}

fn main() {
    let config = Config::parse();
    let _ = config.clean;
}
```
playground: https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=ba6ae5eae9e9514d276dc40d5479cf5e

### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

At runtime, clap panics with the following message:
```
thread 'main' panicked at /playground/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.19/src/builder/debug_asserts.rs:696:9:
assertion `left == right` failed: Argument `clean`'s selected action Count contradicts `value_parser` (ValueParser::other(core::option::Option<playground::CleanLevel>))
  left: u8
 right: core::option::Option<playground::CleanLevel>
```

### Expected Behaviour

I would expect that the value parser is called to produce a `u8` and then `CleanLevel::from_val` is called on the parsed value to map to an `Option<CleanLevel>`. It seems the sequence of events in parsing is somehow wrong, or maybe I'm misunderstanding how `MapValueParser` works.

### Additional Context

The alternative solution is to make use of an intermediate struct and a method on that struct, like so:

```rust
use clap::{ArgAction, Args, Parser};

#[derive(Parser)]
struct Config {
    #[clap(flatten)]
    clean: Clean,
}

#[derive(Args, Clone)]
struct Clean {
    #[clap(short = 'c', action = ArgAction::Count)]
    count: u8
}

impl Clean {
    fn level(&self) -> Option<CleanLevel> {
        match self.count {
            0 => None,
            1 => Some(CleanLevel::Untracked),
            2.. => Some(CleanLevel::All),
        }
    }
}

enum CleanLevel {
    Untracked,
    All,
}

fn main() {
    let config = Config::parse();
    let _ = config.clean.level();
}
```

However, this introduces extra clutter and indirection. It would be great to avoid using an intermediate representation.

### Debug Output

_No response_

---

_Label `C-bug` added by @mkrasnitski on 2024-11-02 07:45_

---

_Comment by @epage on 2024-11-04 15:35_

When you pass `clap::value_parser!(u8).map(CleanLevel::from_val` to clap, you are passing in an adapter from an `OsStr` to a `CleanLevel`.  The intermediate `u8` is transient and never shared with clap and  clap doesn't know how to increment `CleanLevel`.

For this to work, we'd need to do one of the following
- Have a derive-specific adapter (we had this with clap v3 and that was what value_parser was working to replace)
- We build into the concept of our value parser the ability to increment, with it erroring by default.  However, this is a fairly specialized operation and doesn't scale to other needs
- We allow value parsers to mutate the collection the value gets stored into, either appending or incrementing as needed.  This would be a major re-work and would require some very careful `dyn` trait design to allow all of the interactions to play  out.

As for workarounds...

We take the "intermediate" struct approach in https://docs.rs/clap-verbosity-flag/latest/clap_verbosity_flag/

The conversion can also be a helper method on `Config` which I do for other types of data in my CLIs.

---

_Comment by @mkrasnitski on 2024-11-04 17:52_

> The intermediate `u8` is transient and never shared with clap and clap doesn't know how to increment `CleanLevel`.

This seems solvable by introducing a trait called something like `Counter`, with an `increment` method, like you mentioned. In the case of `CleanLevel`, the state machine implements a kind of saturating addition, and auto-implementing this functionality would be hard via `#[derive(Counter)]`, but a manual implementation would work fine. Default implementations for integers and potentially one for `Option<T>` (`0 => None`) would work well IMO.

> However, this is a fairly specialized operation and doesn't scale to other needs

Could you elaborate on this point? Yes, likely this new trait would be closely coupled to `ArgAction::Count`, but that could be made clear with a good name. Arguably this is a relatively simple generalization to make to allow using arbitrary types as counters, enabling better state machines while staying pretty self-contained.



---

_Comment by @epage on 2024-11-04 18:01_

> This seems solvable by introducing a trait called something like Counter, with an increment method, like you mentioned. In the case of CleanLevel, the state machine implements a kind of saturating addition, and auto-implementing this functionality would be hard via #[derive(Counter)], but a manual implementation would work fine. Default implementations for integers and potentially one for Option<T> (0 => None) would work well IMO.

We are effectively operating on a `Box<Any>` for the intermediate state due to the dynamic nature of clap.  We can't easily access a trait like `Counter`.

> Could you elaborate on this point? Yes, likely this new trait would be closely coupled to ArgAction::Count, but that could be made clear with a good name. Arguably this is a relatively simple generalization to make to allow using arbitrary types as counters, enabling better state machines while staying pretty self-contained.

I was specifically referring to baking in the information for this behavior in `ValueParser` which is what has knowledge of the concrete type.  This is unrelated to a "new trait" solution.

---
