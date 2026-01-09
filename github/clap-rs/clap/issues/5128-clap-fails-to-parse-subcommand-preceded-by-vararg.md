---
number: 5128
title: Clap fails to parse subcommand preceded by vararg 
type: issue
state: closed
author: Pzixel
labels:
  - C-bug
assignees: []
created_at: 2023-09-15T14:51:00Z
updated_at: 2023-09-18T17:37:18Z
url: https://github.com/clap-rs/clap/issues/5128
synced_at: 2026-01-07T13:12:20-06:00
---

# Clap fails to parse subcommand preceded by vararg 

---

_Issue opened by @Pzixel on 2023-09-15 14:51_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.72.0-nightly (fe7454bf4 2023-06-19)

### Clap Version

4.4.3

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};
use clap::ValueEnum;

#[derive(Debug, Clone, Copy, clap::ValueEnum, derive_more::Display, Eq, PartialEq)]
#[clap(rename_all = "verbatim")]
pub enum Flags {
    Hello,
    World
}

#[derive(Debug, Clone, Copy, Eq, PartialEq, Subcommand)]
pub enum SubCommand {
    #[clap(name = "run-once")]
    Once,
    #[clap(name = "run-regular")]
    Regular
}


#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
pub struct Args {
    // value_variants
    #[arg(short, long, num_args(0..), default_values_t = Flags::value_variants())]
    pub flags: Vec<Flags>,
    #[command(subcommand)]
    pub subcommand: SubCommand,
}

fn main() {
    let args = Args::parse();
    dbg!(args);
}

```


### Steps to reproduce the bug with the above code

`cargo run -- --flags Hello World run-once`

### Actual Behaviour

```
error: invalid value 'run-once' for '--flags [<FLAGS>...]'
  [possible values: Hello, World]
```

### Expected Behaviour

It should run subcommand `Once` with flags `Hello`

### Additional Context

_No response_

### Debug Output

```rust
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="untitled90"
[clap_builder::builder::command]Command::_propagate:untitled90
[clap_builder::builder::command]Command::_check_help_and_version:untitled90 expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:untitled90
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:flags
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:version
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--flags"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--flags")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--flags [<FLAGS>...]'
[clap_builder::parser::parser]Parser::parse_long_arg("flags"): Found an arg with value 'None'
[clap_builder::parser::parser]Parser::parse_opt_value; arg=flags, val=None, has_eq=false
[clap_builder::parser::parser]Parser::parse_opt_value; arg.settings=ArgFlags(0)
[clap_builder::parser::parser]Parser::parse_opt_value; Checking for val...
[clap_builder::parser::parser]Parser::parse_opt_value: More arg vals required...
[clap_builder::parser::parser]Parser::get_matches_with: After parse_long_arg Opt("flags")
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"Hello"'
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=flags, pending=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=0..=18446744073709551615, actual=1
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"run-once"'
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=flags, pending=2
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=0..=18446744073709551615, actual=2
[clap_builder::parser::parser]Parser::resolve_pending: id="flags"
[clap_builder::parser::parser]Parser::react action=Append, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Parser::react: cur_idx:=1
[clap_builder::parser::parser]Parser::remove_overrides: id="flags"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="flags", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="flags"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Args", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["Hello", "run-once"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=3
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: invalid value 'run-once' for '--flags [<FLAGS>...]'
  [possible values: Hello, World]
```

---

_Label `C-bug` added by @Pzixel on 2023-09-15 14:51_

---

_Comment by @epage on 2023-09-18 13:55_

You might want https://docs.rs/clap/latest/clap/struct.Command.html#method.subcommand_precedence_over_arg

---

_Comment by @Pzixel on 2023-09-18 14:35_

Hmm, it looks like it should help, but I didn't manage to find a usage in attributes. Can you please show an example that resolves the issue? Either I'm doing it wrong or it doesn't resolve this issue:

```rust
#[derive(Debug, Clone, Copy, Eq, PartialEq, Subcommand)]
pub enum SubCommand {
    #[clap(name = "run-once", subcommand_precedence_over_arg = true)]
    Once,
    #[clap(name = "run-regular", subcommand_precedence_over_arg = true)]
    Regular
}

error: invalid value 'run-once' for '--flags [<FLAGS>...]'
  [possible values: Hello, World]

```

---

_Comment by @epage on 2023-09-18 16:05_

This is an attribute that would go on the parent command for determining how to parse all subcommands.

---

_Comment by @Pzixel on 2023-09-18 17:37_

I see, thank you.

---

_Closed by @Pzixel on 2023-09-18 17:37_

---
