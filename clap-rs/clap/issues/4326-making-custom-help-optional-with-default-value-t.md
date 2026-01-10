---
number: 4326
title: making custom help optional with default_value_t panicks
type: issue
state: closed
author: erazor-de
labels:
  - C-bug
assignees: []
created_at: 2022-10-01T07:41:53Z
updated_at: 2022-10-01T18:58:31Z
url: https://github.com/clap-rs/clap/issues/4326
synced_at: 2026-01-10T01:27:53Z
---

# making custom help optional with default_value_t panicks

---

_Issue opened by @erazor-de on 2022-10-01 07:41_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0 (a55dd71d5 2022-09-19)

### Clap Version

4.0.7

### Minimal reproducible code

```
[package]
name = "claptest"
version = "0.1.0"
edition = "2021"

[dependencies]
clap = { version = "4", features = ["derive", "debug"] }
```

```rust
use clap::{ArgAction, Parser};

#[derive(Parser)]
#[command(disable_help_flag = true)]
struct Args {
    #[arg(long, action = ArgAction::Help, default_value_t = false)]
    help: bool,
}

fn main() {
    let _args = Args::parse();
}
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

> thread 'main' panicked at 'assertion failed: `(left == right)`
  left: `["false"]`,
 right: `[]`', /home/stefan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.7/src/parser/parser.rs:1277:17

### Expected Behaviour

No panick, no output.

### Additional Context

I try to get rid of the short help '-h' by removing default behaviour and creating own help argument with only long version using default functionality.
With using only `#[arg(long, action = ArgAction::Help)]` help is required.
Making the argument optional with `default_value_t = false` leads to panick.
Using `help: Option<bool>,` works and can be used as workaround.

### Debug Output

> [      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="claptest"
[      clap::builder::command]  Command::_propagate:claptest
[      clap::builder::command]  Command::_check_help_and_version:claptest expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_propagate_global_args:claptest
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::add_defaults
[        clap::parser::parser]  Parser::add_defaults:iter:help:
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:help: has default vals
[        clap::parser::parser]  Parser::add_default_value:iter:help: wasn't used
[        clap::parser::parser]  Parser::react action=Help, identifier=None, source=DefaultValue
thread 'main' panicked at 'assertion failed: `(left == right)`
  left: `["false"]`,
 right: `[]`', /home/stefan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.7/src/parser/parser.rs:1277:17
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


---

_Label `C-bug` added by @erazor-de on 2022-10-01 07:41_

---

_Referenced in [clap-rs/clap#4330](../../clap-rs/clap/pulls/4330.md) on 2022-10-01 18:37_

---

_Closed by @epage on 2022-10-01 18:51_

---

_Comment by @epage on 2022-10-01 18:58_

The assert is removed in 4.0.8.  I've also considered adding support for `()` as a type to say "don't bother reading a value".  If you are interested in that for this case, feel free to create an issue

---
