---
number: 5690
title: Global argument clashes with subcommand positional of same name
type: issue
state: closed
author: bennetthardwick
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2024-08-21T06:03:01Z
updated_at: 2024-08-21T14:22:55Z
url: https://github.com/clap-rs/clap/issues/5690
synced_at: 2026-01-07T13:12:20-06:00
---

# Global argument clashes with subcommand positional of same name

---

_Issue opened by @bennetthardwick on 2024-08-21 06:03_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.78.0 (9b00956e5 2024-04-29)

### Clap Version

4.5.16

### Minimal reproducible code

```rust
use clap::{ Subcommand, Parser }; // v4.5.16

#[derive(Subcommand)]
enum Sub {
    Test {
        field_name: u64
    }
}

#[derive(Parser)]
struct Cli {
    #[command(subcommand)]
    cmd: Sub,

    #[arg(long, global = true)]
    field_name: Option<String>
}


fn main() {
    let args = Cli::parse_from(["cmd", "test", "42"]);
    drop(args);
}
```

[Playground Link](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=6f01d8ed862825ad817ca73d72d48f2a)


### Steps to reproduce the bug with the above code

```sh
cargo run
```

### Actual Behaviour

When I run this I get the following error:

```txt
Mismatch between definition and access of `field_name`. Could not downcast to alloc::string::String, need to downcast to u64
```

### Expected Behaviour

It should either return a compile error due to the duplicate field name or parse correctly.

### Additional Context

_No response_

### Debug Output

```txt
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-test"
[clap_builder::builder::command]Command::_propagate:clap-test
[clap_builder::builder::command]Command::_check_help_and_version:clap-test expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:clap-test
[clap_builder::builder::command]Command::_propagate skipping "field_name" to test, already exists
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:field_name
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"test"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("test")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("test")
[clap_builder::parser::parser]Parser::parse_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of test to "cmd test"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of test to "clap-test-test"
[clap_builder::builder::command]Command::_build: name="test"
[clap_builder::builder::command]Command::_propagate:test
[clap_builder::builder::command]Command::_check_help_and_version:test expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:test
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:field_name
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=test
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"42"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("42")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]Parser::resolve_pending: id="field_name"
[clap_builder::parser::parser]Parser::react action=Set, identifier=Some(Index), source=CommandLine
[clap_builder::parser::parser]Parser::remove_overrides: id="field_name"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="field_name", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="field_name"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Test", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["42"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=field_name, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:field_name:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:field_name: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="field_name"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="field_name", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="Test", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="field_name"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="field_name"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([Child { id: "field_name", children: [] }])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"field_name"
[clap_builder::parser::validator]Validator::gather_requires:iter:"Test"
[clap_builder::parser::validator]Validator::gather_requires:iter:"Test":group
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:field_name:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:field_name: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::arg_matcher]ArgMatcher::get_global_values: global_arg_vec=["field_name"]
```

---

_Label `C-bug` added by @bennetthardwick on 2024-08-21 06:03_

---

_Label `A-derive` added by @epage on 2024-08-21 14:17_

---

_Comment by @epage on 2024-08-21 14:22_

When parsing the subcommand, we need a way to refer to both the parent argument and the subcommand's argument.  Due to limitations in derives, to either make this automatic or error, we'd have to handle this at runtime through the design of the derive traits which would require a lot of complex logic to be generated and we'd need to constantly be making breaking changes on the traits or have no stability guarantees on them.    This is being tracked in #3133.

#4701 would reduce the chance of hitting this problem.

Closing in favor of those two issues.  If there is a reason we should keep this open separately, let us know!

---

_Closed by @epage on 2024-08-21 14:22_

---
