---
number: 3281
title: Positional arg after repeated positional arg - prefer compiler error to runtime error
type: issue
state: closed
author: nipunn1313
labels:
  - C-enhancement
  - A-derive
  - S-duplicate
assignees: []
created_at: 2022-01-11T18:06:02Z
updated_at: 2022-01-12T16:43:39Z
url: https://github.com/clap-rs/clap/issues/3281
synced_at: 2026-01-07T13:12:19-06:00
---

# Positional arg after repeated positional arg - prefer compiler error to runtime error

---

_Issue opened by @nipunn1313 on 2022-01-11 18:06_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.6

### Describe your use case

```rust
use clap::Parser;

#[derive(clap::Parser, Debug)]
struct Args {
    arg2: Vec<String>,
    arg1: String,
}

fn main() {
    let args = Args::parse();
    dbg!(args);
}
```

This code is bad. Currently when running it, it fails at runtime
```
➜  clap_repro cargo run
   Compiling clap-repro v0.1.0 (/Users/nipunn/src/clap_repro)
    Finished dev [unoptimized + debuginfo] target(s) in 0.30s
     Running `target/debug/clap-repro`
thread 'main' panicked at 'Found non-required positional argument with a lower index than a required positional argument: "arg2" index Some(1)', /Users/nipunn/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.6/src/parse/parser.rs:210:21
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

### Describe the solution you'd like

If possible, some kind of compile time error.
Probably would involve a proc-macro to iterate/logic over the struct and bail

### Alternatives, if applicable

Status quo runtime error could work, but means you don't catch clear bugs until much later.
If we stick with status quo, a better error message might be nice.

"Found positional argument after a repeated positional argument"

### Additional Context

_No response_

---

_Label `C-enhancement` added by @nipunn1313 on 2022-01-11 18:06_

---

_Comment by @epage on 2022-01-11 18:13_

#2720 is about supporting all debug asserts in the derive macro.   As noted in that issue, there are complications with that.  This falls into some of those complications because of `flatten` making it so we can't fully reason about this case.  At best, we can implement a compile error only for when a `flatten` is not used, leaving it to runtime otherwise.

As noted in #2720, our [CHANGELOG](https://github.com/clap-rs/clap/blob/master/CHANGELOG.md#300---2021-12-31) and Derive Tutorial's [tips](https://github.com/clap-rs/clap/tree/master/examples/tutorial_derive#tips), we encourage having a test like below so these will be caught while testing (and for all subcommands, when relevant) as compared to manual testing / using the application.

```rust
#[derive(Parser)]
#[clap(author, version, about, long_about = None)]
struct Cli {
    ...
}

#[test]
fn verify_app() {
    use clap::IntoApp;
    Cli::into_app().debug_assert()
}
```

So like #2720, I lean towards closing this issue out as "won't fix".  If there are thoughts or concerns about this, let us know!

---

_Closed by @epage on 2022-01-11 18:13_

---

_Label `A-derive` added by @epage on 2022-01-11 18:20_

---

_Label `S-duplicate` added by @epage on 2022-01-11 18:20_

---

_Comment by @nipunn1313 on 2022-01-11 23:16_

Sounds good. May still be nice to have a better runtime error message!
```
thread 'main' panicked at 'Found non-required positional argument with a lower index than a required positional argument: "arg2" index Some(1)'
```

was somewhat misleading to us. To my user, it seemed like it was user-error for providing the args in the wrong order (or something like that) - when actually it was a bug in the specification of the CLI by me.

---

_Comment by @nipunn1313 on 2022-01-11 23:29_

I also don't think `debug_assert()` catches this issue

Here's a `cargo play` compatible snippit - drop it into a `main.rs`. (`cargo install cargo-play ; cargo play main.rs`)
```rust
//# clap = {version="3", features=["derive"]}
use clap::{IntoApp, Parser};

#[derive(clap::Parser, Debug)]
struct Args {
    arg2: Vec<String>,
    arg1: String,
}

fn main() {
    Args::into_app().debug_assert();
    let args = Args::parse();
    dbg!(args);
}
```

If I run it, the debug_assert doesn't catch the issue, but Args::parse() fails at runtime.

---

_Comment by @epage on 2022-01-12 16:43_

Interesting!  This lives in a completely different place but doesn't need to.  My guess is it predates some of how we do assertions today.

---

_Referenced in [clap-rs/clap#3288](../../clap-rs/clap/pulls/3288.md) on 2022-01-12 16:56_

---
