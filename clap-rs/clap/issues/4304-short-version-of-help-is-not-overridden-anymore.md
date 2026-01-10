---
number: 4304
title: short version of help is not overridden anymore
type: issue
state: closed
author: erazor-de
labels:
  - C-bug
assignees: []
created_at: 2022-09-30T17:03:28Z
updated_at: 2022-09-30T18:08:39Z
url: https://github.com/clap-rs/clap/issues/4304
synced_at: 2026-01-10T01:27:53Z
---

# short version of help is not overridden anymore

---

_Issue opened by @erazor-de on 2022-09-30 17:03_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0 (a55dd71d5 2022-09-19)

### Clap Version

4.0.6

### Minimal reproducible code

```
[package]
name = "claptest"
version = "0.1.0"
edition = "2021"

[dependencies]
clap = { version = "4", features = ["derive"] }
```

```rust
use clap::Parser;

#[derive(Parser)]
pub struct Args {
    #[arg(short, long)]
    pub hyperlink: bool,
}

fn main() {
    let _args = Args::parse();
}
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

Panicks with following message:

> thread 'main' panicked at 'Command claptest: Short option names must be unique for each argument, but '-h' is in use by both 'hyperlink' and 'help' (call `cmd.disable_help_flag(true)` to remove the auto-generated `--help`)', /home/stefan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.6/src/builder/debug_asserts.rs:121:17

### Expected Behaviour

clap version 3 removed the short help '-h' when another arg used it.

### Additional Context

_No response_

### Debug Output

```
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="claptest"
[      clap::builder::command]  Command::_propagate:claptest
[      clap::builder::command]  Command::_check_help_and_version:claptest expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_propagate_global_args:claptest
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:hyperlink
thread 'main' panicked at 'Command claptest: Short option names must be unique for each argument, but '-h' is in use by both 'hyperlink' and 'help' (call `cmd.disable_help_flag(true)` to remove the auto-generated `--help`)', /home/stefan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.6/src/builder/debug_asserts.rs:121:17
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Label `C-bug` added by @erazor-de on 2022-09-30 17:03_

---

_Comment by @shepmaster on 2022-09-30 18:01_

The section "Removing Implicit Version/Help Behavior" in [clap 4.0, a Rust CLI argument parser](https://epage.github.io/blog/2022/09/clap4/) may be relevant.

---

_Comment by @epage on 2022-09-30 18:08_

Yes,. this was an intentional change and the above linked blog post provides a summary of it and why.

We covered parts of the change in the CHANGELOG,

> mut_arg can no longer be used to customize help and version arguments, instead disable them (Command::disable_help_flag, Command::disable_version_flag) and provide your own (#4056)

and 

> Arg::new("help") and Arg::new("version") no longer implicitly disable the built-in flags and be copied to all subcommands, instead disable the built-in flags (Command::disable_help_flag, Command::disable_version_flag) and mark the custom flags as global(true). (#4056)

but we seemed to have not explicitly called out this aspect of it.  That has no been rectified.

As this is expected behavior and (now) documented, I'm closing this out. 

---

_Closed by @epage on 2022-09-30 18:08_

---

_Referenced in [clap-rs/clap#4311](../../clap-rs/clap/issues/4311.md) on 2022-09-30 19:19_

---
