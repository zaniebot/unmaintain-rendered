---
number: 4837
title: Automatically detect allow_missing_positional
type: issue
state: open
author: joshtriplett
labels:
  - C-enhancement
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2023-04-17T04:35:10Z
updated_at: 2023-10-25T02:26:44Z
url: https://github.com/clap-rs/clap/issues/4837
synced_at: 2026-01-07T13:12:20-06:00
---

# Automatically detect allow_missing_positional

---

_Issue opened by @joshtriplett on 2023-04-17 04:35_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.2.2

### Describe your use case

I'd like to make a CLI that accepts either "arg1 arg2" or just "arg2" (the first argument is optional).

I wrote this:

```rust
#[derive(clap::Args, Debug)]
#[command(arg_required_else_help = true)]
pub struct MySubcommand {
    /// ...
    pub name: Option<String>,

    /// ...
    pub dir: PathBuf,

    /// ...
}
```

This *did* cause clap to realize that `name` should be optional, based on the generated help:

```
subcommand description

Usage: cmd subcmd [OPTIONS] [NAME] <DIR>

Arguments:
  [NAME]  ...
  <DIR>   ...
```

Notice the `[NAME]` vs `<DIR>`.

However, if I actually *run* the command with one argument:

```
error: the following required arguments were not provided:
  <DIR>

Usage: cmd subcmd <NAME> <DIR>
```

I had to supply `#[command(allow_missing_positional = true)]` to make this work as expected.

### Describe the solution you'd like

I'd like clap to automatically assume that if it has a single positional argument specified with `Option`, it should assume `allow_missing_positional = true`.

### Alternatives, if applicable

If there's a strong reason *not* to autodetect this, then instead, I'd suggest producing an error on an `Option` positional argument with `allow_missing_positional = false`.

However, if at all possible I'd suggest making this work automatically.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @joshtriplett on 2023-04-17 04:35_

---

_Label `A-derive` added by @epage on 2023-04-17 15:08_

---

_Label `S-waiting-on-design` added by @epage on 2023-04-17 15:08_

---

_Comment by @epage on 2023-04-17 15:28_

The challenges this would run into
- (either solution) Technically you could flatten in positionals from different structs, making this impossible to do with `flatten` and I've been loathe to offer additional features only for the non-flatten cases (see also #3133)
- (unlikely but ...) a developer could set `index` to an expression and we would be in a similar opaque situation as `flatten`
- (unlikely but ....) a developer could set `index` to a literal, requiring us to start tracking it so we can detect either case.  The more we add to the derive, the slower it is to compile and run, so we need to be judicious in what we add
- There are times when a user might use `Option` but set `#[arg(required = true)]` (e.g. a conflict with all positionals).  While #2621 will improve this, people might still want to do the current workaround.  This ends up being another case of `index` of dealing with expressions and literals
- (a bit hand waive-y) `allow_missing_positional` is not very polished at this time (seems like people are regularly running into limitations) and the more we streamline it without polish, the more people are likely to use it and get frustrated at the limitations

None of this is to say "no".  This case is a bit more peculiar than something like `short`, making it less likely to run into problems but I currently lean towards "no" for a simpler, more predictable experience.

---

_Comment by @epage on 2023-04-17 15:29_

> This did cause clap to realize that name should be optional, based on the generated help:

This should have caused a debug assert to fire

```rust
#!/usr/bin/env cargo-eval

//! ```cargo
//! [dependencies]
//! clap = { version = "4", features = ["derive"] }
//! ```

use clap::{Args, Parser, Subcommand};
use std::path::PathBuf;

#[derive(Parser, Debug)]
#[command(arg_required_else_help = true)]
pub struct Cli {
    /// ...
    pub name: Option<String>,

    /// ...
    pub dir: PathBuf,
}

fn main() {
    let cli = Cli::parse();
    dbg!(cli);
}
```
```
thread 'main' panicked at 'Found non-required positional argument with a lower index than a required positional argument: "name" index Some(1)', /home/epage/.cargo/registry/src/github.com-1ecc6299db9ec823/clap_builder-4.2.2/src/builder/debug_asserts.rs:639:17
```

I could see us suggesting `last` or `allow_misisng_positional` in the error message.

---

_Comment by @nyurik on 2023-10-24 22:15_

@epage I ran into the same issue when trying to implement `cp` shell command-like interface (any number of sources followed by a single target - `cp src1 src2 src3 destination`). Are there any suggested ways to do this, or is the best to simply implement a single vector of `PathBuf`s, and explain in the help message what those should be? Thx!

---

_Comment by @epage on 2023-10-25 02:26_

atm a single vector of `PathBuf`

---

_Referenced in [clap-rs/clap#6105](../../clap-rs/clap/issues/6105.md) on 2025-08-15 13:50_

---
