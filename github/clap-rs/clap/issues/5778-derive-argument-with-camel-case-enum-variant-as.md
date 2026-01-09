---
number: 5778
title: Derive-argument with camel-case enum variant as default_value_t generates wrong default value
type: issue
state: closed
author: Cydhra
labels:
  - C-bug
assignees: []
created_at: 2024-10-15T14:03:31Z
updated_at: 2024-10-23T11:16:10Z
url: https://github.com/clap-rs/clap/issues/5778
synced_at: 2026-01-07T13:12:20-06:00
---

# Derive-argument with camel-case enum variant as default_value_t generates wrong default value

---

_Issue opened by @Cydhra on 2024-10-15 14:03_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.81.0 (eeb90cda1 2024-09-04)

### Clap Version

4.5.20

### Minimal reproducible code

```rust
use std::fmt::Display;
use clap::{Parser, ValueEnum};

#[derive(Clone, Debug, Parser)]
struct Cli {
    #[arg(short, long, default_value_t = Cmd::AnotherAction)]
    args: Cmd,
}

#[derive(Clone, Debug, ValueEnum)]
enum Cmd {
    Action, AnotherAction
}

impl Display for Cmd {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        write!(f, "{:?}", self)
    }
}

fn main() {
    let _ = Cli::parse();
}

```


### Steps to reproduce the bug with the above code

```
cargo run
```

### Actual Behaviour

Parsing panics with:
```
error: invalid value 'AnotherAction' for '--args <ARGS>'
  [possible values: action, another-action]

  tip: a similar value exists: 'another-action'

For more information, try '--help'.
```

### Expected Behaviour

Since I specify the `default_value_t` in a type-safe manner, I'd expect it to generate a correct default value (`another-action`).

### Additional Context

The wrong default is also visible when printing `--help`:
```
  -a, --args <ARGS>  [default: AnotherAction] [possible values: action, another-action]
```

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-test"
[clap_builder::builder::command]Command::_propagate:clap-test
[clap_builder::builder::command]Command::_check_help_and_version:clap-test expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:clap-test
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:args
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:args:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:args: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:args: wasn't used
[clap_builder::parser::parser]Parser::react action=Set, identifier=None, source=DefaultValue
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="args", source=DefaultValue
[clap_builder::parser::parser]Parser::push_arg_values: ["AnotherAction"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: invalid value 'AnotherAction' for '--args <ARGS>'
  [possible values: action, another-action]

  tip: a similar value exists: 'another-action'

For more information, try '--help'.
```

---

_Label `C-bug` added by @Cydhra on 2024-10-15 14:03_

---

_Comment by @epage on 2024-10-23 11:16_

`default_value_t` is a wrapper around `default_value`.  All of our internal logic deals with command-line-like values.

We rely on `Display` for doing this and if the `Display` doesn't match then it will fail.  You can opt-in to an alternative to `Display with the `value_enum` attribute

```rust
use std::fmt::Display;
use clap::{Parser, ValueEnum};

#[derive(Clone, Debug, Parser)]
struct Cli {
    #[arg(short, long, value_enum, default_value_t = Cmd::AnotherAction)]
    args: Cmd,
}

#[derive(Clone, Debug, ValueEnum)]
enum Cmd {
    Action, AnotherAction
}

impl Display for Cmd {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        write!(f, "{:?}", self)
    }
}

fn main() {
    let _ = Cli::parse();
}
```

Alternative, you can delegate your `Display` to `ValueEnum`, see
https://github.com/clap-rs/clap/blob/61f5ee514f8f60ed8f04c6494bdf36c19e7a8126/examples/git-derive.rs#L68-L75

---

_Closed by @epage on 2024-10-23 11:16_

---
