```yaml
number: 5513
title: "Subcommand Conflicts with Argument (`derive` feature)"
type: issue
state: open
author: lkurcak
labels:
  - C-bug
  - A-parsing
  - A-validators
  - S-waiting-on-design
assignees: []
created_at: 2024-06-01T01:09:41Z
updated_at: 2024-06-13T07:34:28Z
url: https://github.com/clap-rs/clap/issues/5513
synced_at: 2026-01-12T16:14:17Z
```

# Subcommand Conflicts with Argument (`derive` feature)

---

_@lkurcak_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.78.0 (9b00956e5 2024-04-29)

### Clap Version

clap = { version = "4.5.4", features = ["derive"] }

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Debug, Parser)]
struct Cli {
    name: String,
    #[command(subcommand)]
    command: Commands,
}

#[derive(Debug, Subcommand)]
enum Commands {
    Test,
}

fn main() {
    let args = Cli::parse();

    dbg!(&args);
}
```

### Steps to reproduce the bug with the above code

### OK
Positional name argument is `hello`, subcommand is `test`:
```sh
cargo run -- hello test
```
### BAD
Positional name argument is `test`, subcommand is `test`:
```sh
cargo run -- test test
```

### Actual Behaviour

### OK
```sh
cargo run -- hello test
[src\main.rs:18:5] &args = Cli {
    name: "hello",
    command: Test,
}
```
### BAD
```sh
cargo run -- test test
error: unexpected argument 'test' found

Usage: testing.exe <NAME> test

For more information, try '--help'.
```

### Expected Behaviour

Clap should parse the arguments positionally:

```sh
cargo run -- test test
[src\main.rs:18:5] &args = Cli {
    name: "test",
    command: Test,
}
```

### Additional Context

_No response_

### Debug Output

cargo run -- test test
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="testing"
[clap_builder::builder::command]Command::_propagate:testing
[clap_builder::builder::command]Command::_check_help_and_version:testing expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:testing
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:name
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"test"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("test")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("test")
[clap_builder::parser::parser]Parser::parse_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=["name"]
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"name" arg is_present=false
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[StyledStr("<NAME>")]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of test to "testing.exe test"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of test to "testing-test"
[clap_builder::builder::command]Command::_build: name="test"
[clap_builder::builder::command]Command::_propagate:test
[clap_builder::builder::command]Command::_check_help_and_version:test expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:test
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=test
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"test"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("test")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=help
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: testing.exe <NAME> test
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: unexpected argument 'test' found

Usage: testing.exe <NAME> test

For more information, try '--help'.


---

_Label `C-bug` added by @lkurcak on 2024-06-01 01:09_

---

_Label `A-parsing` added by @epage on 2024-06-04 17:38_

---

_Label `A-validators` added by @epage on 2024-06-04 17:38_

---

_Label `S-waiting-on-design` added by @epage on 2024-06-04 17:38_

---

_Comment by @epage on 2024-06-04 17:44_

We have [`subcommand_precedence_over_arg`](https://docs.rs/clap/latest/clap/struct.Command.html#method.subcommand_precedence_over_arg) but that only applies to options; subcommands always have precedence over positionals today (the name makes it sounds more general).

Some potential routes to go

Do nothing, discouraging mixing of positionals being additive with subcommands.  maybe we could add some debug asserts to help with this though that can get complicated with various settings.

We break compatibility and default to positionals having higher precedence, controlled by `subcommand_precedence_over_arg`.

The parser could detect there are unsatisfied requireds and ensure give them precedene.  Like with the debug asserts for the "do nothing" solution, this get complicated due to the various states we can be in

Add a new setting to toggle control over this.  We've generally been working to shrinking the API, rather than growing it.  The more features we add to the API, the less value we get out of them because they become harder to discover and be used.

---

_Comment by @ZakharEl on 2024-06-13 07:30_

I see no reason to do this. derive should not incur any runtime costs for unused features, only compile time. I agree with Lubomirkurcak. This makes your library unuseable for many use cases. 

---

_Comment by @ZakharEl on 2024-06-13 07:34_

I would agree with Epage's case if the arg was an optional arg though.

---

_Referenced in [feldera/feldera#2447](../../feldera/feldera/pulls/2447.md) on 2024-09-08 06:27_

---
