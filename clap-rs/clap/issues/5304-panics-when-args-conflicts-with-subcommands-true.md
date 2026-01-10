---
number: 5304
title: "Panics when `args_conflicts_with_subcommands(true)`"
type: issue
state: closed
author: sorairolake
labels:
  - C-bug
assignees: []
created_at: 2024-01-15T09:37:02Z
updated_at: 2024-01-15T16:20:56Z
url: https://github.com/clap-rs/clap/issues/5304
synced_at: 2026-01-10T01:28:10Z
---

# Panics when `args_conflicts_with_subcommands(true)`

---

_Issue opened by @sorairolake on 2024-01-15 09:37_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.75.0

### Clap Version

4.4.16

### Minimal reproducible code

```rust
use clap::{Args, Parser, Subcommand};

#[derive(Debug, Parser)]
#[command(args_conflicts_with_subcommands(true))]
struct Opt {
    #[arg(short, long)]
    foo: bool,

    #[command(subcommand)]
    command: Option<Command>,
}

#[derive(Debug, Subcommand)]
enum Command {
    Bar(Bar),
}

#[derive(Args, Debug)]
struct Bar {
    #[arg(short, long)]
    baz: bool,
}

fn main() {
    let opt = Opt::parse();
    println!("{opt:?}");
}
```


### Steps to reproduce the bug with the above code

`cargo run -- -f bar`

### Actual Behaviour

```text
thread 'main' panicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.16/src/parser/parser.rs:517:53:
called `Option::unwrap()` on a `None` value
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

### Expected Behaviour

Don't panic.

### Additional Context

_No response_

### Debug Output

```text
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-test"
[clap_builder::builder::command]Command::_propagate:clap-test
[clap_builder::builder::command]Command::_check_help_and_version:clap-test expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:clap-test
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:foo
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-f"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-f")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "f", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['f']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_arg:iter:f
[clap_builder::parser::parser]Parser::parse_short_arg:iter:f: Found valid opt or flag
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=Some(Short), source=CommandLine
[clap_builder::parser::parser]Parser::react: has default_missing_vals
[clap_builder::parser::parser]Parser::remove_overrides: id="foo"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="foo", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="foo"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Opt", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["true"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::get_matches_with: After parse_short_arg ValuesDone
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"bar"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("bar")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
thread 'main' panicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.16/src/parser/parser.rs:517:53:
called `Option::unwrap()` on a `None` value
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Label `C-bug` added by @sorairolake on 2024-01-15 09:37_

---

_Referenced in [clap-rs/clap#5306](../../clap-rs/clap/pulls/5306.md) on 2024-01-15 15:33_

---

_Closed by @epage on 2024-01-15 16:19_

---

_Comment by @epage on 2024-01-15 16:20_

Sorry about that.  Should be fixed in 4.4.17

---
