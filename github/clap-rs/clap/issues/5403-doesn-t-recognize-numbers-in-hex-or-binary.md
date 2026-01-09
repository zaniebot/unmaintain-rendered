---
number: 5403
title: "doesn't recognize numbers in hex or binary notation"
type: issue
state: closed
author: zahash
labels:
  - C-enhancement
  - A-parsing
  - A-validators
assignees: []
created_at: 2024-03-20T11:55:14Z
updated_at: 2024-03-22T11:40:52Z
url: https://github.com/clap-rs/clap/issues/5403
synced_at: 2026-01-07T13:12:20-06:00
---

# doesn't recognize numbers in hex or binary notation

---

_Issue opened by @zahash on 2024-03-20 11:55_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.76.0

### Clap Version

4.5.3

### Minimal reproducible code

i want to make a cli tool using clap to convert between different number formats. like binary to decimal, hex to binary, etc... 
but clap cannot recognize numbers in binary or hex notation as usize. it thinks they are strings instead.

```sh
$ cargo run -- bin 0x0F

error: invalid value '0x0F' for '<INPUT>': invalid digit found in string
```

```rust
#[derive(Parser, Debug)]
enum Tools {
    Hex { input: usize },
    Bin { input: usize },
    Dec { input: usize },
}

fn main() {
    match Tools::parse() {
        Tools::Hex { input } => println!("{:X}", input),
        Tools::Bin { input } => println!("{:b}", input),
        Tools::Dec { input } => println!("{}", input),
    }
}
```

### Steps to reproduce the bug with the above code

cargo run -- bin 0x0F

### Actual Behaviour

it thinks 0xF0 is a string

### Expected Behaviour

it should understand that 0xF0 is a usize

### Additional Context

_No response_

### Debug Output

```sh
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="vergil"
[clap_builder::builder::command]Command::_propagate:vergil
[clap_builder::builder::command]Command::_check_help_and_version:vergil expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:vergil
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"bin"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("bin")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("bin")
[clap_builder::parser::parser]Parser::parse_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of bin to "vergil bin"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of bin to "vergil-bin"
[clap_builder::builder::command]Command::_build: name="bin"
[clap_builder::builder::command]Command::_propagate:bin
[clap_builder::builder::command]Command::_check_help_and_version:bin expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:bin
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:input
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=bin
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"0x0F"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("0x0F")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]Parser::resolve_pending: id="input"
[clap_builder::parser::parser]Parser::react action=Set, identifier=Some(Index), source=CommandLine
[clap_builder::parser::parser]Parser::remove_overrides: id="input"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="input", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="input"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Bin", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["0x0F"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
```

---

_Label `C-bug` added by @zahash on 2024-03-20 11:55_

---

_Label `A-parsing` added by @epage on 2024-03-20 14:08_

---

_Label `A-validators` added by @epage on 2024-03-20 14:08_

---

_Label `C-bug` removed by @epage on 2024-03-20 14:08_

---

_Label `C-enhancement` added by @epage on 2024-03-20 14:08_

---

_Comment by @epage on 2024-03-20 14:26_

Right now the parsing is fairly basic with the workaround of providing your own value parser.  Examples of reusable value parsers include
- https://crates.io/crates/clap-num (relevant to your use case)
- https://crates.io/crates/clap-digest
- https://crates.io/crates/clap-stdin
- https://crates.io/crates/clio

This is simplified by the fact that we `impl TypedValueParser for Fn(..) -> ...`.

I am tempted to lean towards keeping things basic
- It is easy to add more on your own
- This would increase compile times / binary size
- Other people might not want it by default

---

_Comment by @zahash on 2024-03-20 14:33_

@epage 

how do i provide my own value parser? because right now i'm doing this.

```rust
#[derive(Parser, Debug)]
enum Cli {
    Hex { input: String },
    Bin { input: String },
    Dec { input: String },
}

#[derive(Debug)]
enum CliError {
    InvalidNum,
}

impl Cli {
    fn run(self) -> Result<(), CliError> {
        match self {
            Cli::Hex { input } => println!("{:X}", parse(&input)?),
            Cli::Bin { input } => println!("{:b}", parse(&input)?),
            Cli::Dec { input } => println!("{}", parse(&input)?),
        }
        Ok(())
    }
}

fn parse(s: &str) -> Result<BigUint, CliError> {
    match s.get(0..2) {
        Some("0x") => <BigUint as Num>::from_str_radix(&s[2..], 16).map_err(|_| CliError::InvalidNum),
        Some("0b") => <BigUint as Num>::from_str_radix(&s[2..], 2).map_err(|_| CliError::InvalidNum),
        _ => <BigUint as Num>::from_str_radix(&s, 10).map_err(|_| CliError::InvalidNum),
    }
}

fn main() -> Result<(), CliError> {
    Cli::parse().run()
}
```

it would be nice if i can somehow hook the `fn parse(s: &str) -> Result<BigUint, CliError>` function into the Clap machinery. so i can just define it like this

```rust
#[derive(Parser, Debug)]
enum Cli {
    Hex { input: BigUint },
    Bin { input: BigUint },
    Dec { input: BigUint },
}
```

---

_Comment by @epage on 2024-03-20 14:44_

```rust
#[derive(Parser, Debug)]
enum Cli {
    Hex {
        #[arg(value_parser = num_parser)]
        input: BigUint
    },
    ...
}

fn num_parser(s: &str) -> Result<BigUint, String> {
    ...
}
```


---

_Comment by @zahash on 2024-03-20 14:58_

@epage 

wow. works nicely. thanks. this issue can be closed.

for future readers, this is the final code

```rust
use clap::Parser;
use num_bigint::BigUint;
use num_traits::Num;

#[derive(Parser, Debug)]
enum Cli {
    Hex {
        #[arg(value_parser = num_parser)]
        input: BigUint,
    },
    Bin {
        #[arg(value_parser = num_parser)]
        input: BigUint,
    },
    Dec {
        #[arg(value_parser = num_parser)]
        input: BigUint,
    },
}

fn num_parser(s: &str) -> Result<BigUint, CliError> {
    match s.get(0..2) {
        Some("0x") => {
            <BigUint as Num>::from_str_radix(&s[2..], 16).map_err(|_| CliError::InvalidNum)
        }
        Some("0b") => {
            <BigUint as Num>::from_str_radix(&s[2..], 2).map_err(|_| CliError::InvalidNum)
        }
        _ => <BigUint as Num>::from_str_radix(&s, 10).map_err(|_| CliError::InvalidNum),
    }
}

#[derive(thiserror::Error, Debug)]
enum CliError {
    #[error("invalid number")]
    InvalidNum,
}

impl Cli {
    fn run(self) -> Result<(), CliError> {
        match self {
            Cli::Hex { input } => println!("{:X}", input),
            Cli::Bin { input } => println!("{:b}", input),
            Cli::Dec { input } => println!("{}", input),
        }
        Ok(())
    }
}

fn main() -> Result<(), CliError> {
    Cli::parse().run()
}

```

---

_Comment by @epage on 2024-03-22 11:40_

As zahash  need is resolved and I lean towards this not being built-in, I'm going to go ahead and close.  If there is a reason for us to re-evaluate, let us know!

---

_Closed by @epage on 2024-03-22 11:40_

---
