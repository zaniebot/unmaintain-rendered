---
number: 5435
title: "`flatten` with `required` breaks requirement checking"
type: issue
state: closed
author: nihaals
labels:
  - C-bug
assignees: []
created_at: 2024-03-28T18:39:13Z
updated_at: 2024-03-28T19:29:08Z
url: https://github.com/clap-rs/clap/issues/5435
synced_at: 2026-01-07T13:12:20-06:00
---

# `flatten` with `required` breaks requirement checking

---

_Issue opened by @nihaals on 2024-03-28 18:39_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.77.1 (7cf61ebde 2024-03-27)

### Clap Version

4.5.4

### Minimal reproducible code

```rust
use clap::{Args, Parser, Subcommand};

#[derive(Parser)]
struct Cli {
    #[command(subcommand)]
    command: Commands,
}

#[derive(Subcommand)]
enum Commands {
    Test {
        #[command(flatten)]
        targets: Target,
    },
}

#[derive(Args)]
#[group(required = true)]
struct Target {
    #[arg(long)]
    all: bool,

    #[command(flatten)]
    items: Items,
}

#[derive(Args)]
#[group(multiple = true)]
struct Items {
    #[arg(long)]
    foo: bool,

    #[arg(long)]
    bar: bool,
}

fn main() {
    Cli::parse();
}
```

### Steps to reproduce the bug with the above code

- `cargo run -- test --help`
- `cargo run -- test`
- `cargo run -- test --all`
- `cargo run -- test --foo --bar`
- `cargo run -- test --all --foo`

### Actual Behaviour

```
error: the following required arguments were not provided:
  <>
```

### Expected Behaviour

Passing in `--all` should satisfy the requirement and it shouldn't show an empty list of required arguments.

### Additional Context

The goal is to either require an individual item or `--all`. So `test` fails, `test --all` and `test --foo --bar` works and `test --all --foo` fails.

This is possible by avoiding `flatten` and using conflicts instead:
```rust
#[derive(Args)]
#[group(required = true)]
struct Target {
    #[arg(long)]
    all: bool,

    #[arg(long, conflicts_with = "all")]
    foo: bool,

    #[arg(long, conflicts_with = "all")]
    bar: bool,
}
```

I couldn't figure out a way to use conflicts as a workaround with `flatten` if the group is still required.

Also an extra question, I'm unsure if this is the best way to have a flag that essentially sets the values of a set of other flags (without checking if `all` is true and then mutating `foo` and `bar` to true/always checking if `foo` or `all` is true). It seems like setting a custom `action` wouldn't help here (to set `foo` and `bar` to true) but maybe there's a better way of setting up the flags here that avoids this issue.

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="binary"
[clap_builder::builder::command]Command::_propagate:binary
[clap_builder::builder::command]Command::_check_help_and_version:binary expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:binary
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:version
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"test"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("test")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("test")
[clap_builder::parser::parser]Parser::parse_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of test to "binary test"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of test to "binary-test"
[clap_builder::builder::command]Command::_build: name="test"
[clap_builder::builder::command]Command::_propagate:test
[clap_builder::builder::command]Command::_check_help_and_version:test expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:test
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:all
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:foo
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:bar
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=test
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--all"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--all")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--all'
[clap_builder::parser::parser]Parser::parse_long_arg("all"): Presence validated
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Parser::react: has default_missing_vals
[clap_builder::parser::parser]Parser::remove_overrides: id="all"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="all", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="all"
[clap_builder::parser::parser]Parser::push_arg_values: ["true"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::get_matches_with: After parse_long_arg ValuesDone
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:all:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:all: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:all: was used
[clap_builder::parser::parser]Parser::add_defaults:iter:foo:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:foo: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:foo: wasn't used
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=None, source=DefaultValue
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="foo", source=DefaultValue
[clap_builder::parser::parser]Parser::push_arg_values: ["false"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::parser]Parser::add_defaults:iter:bar:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:bar: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:bar: wasn't used
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=None, source=DefaultValue
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="bar", source=DefaultValue
[clap_builder::parser::parser]Parser::push_arg_values: ["false"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=3
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="all"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="all", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="all"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="all"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([Child { id: "Target", children: [] }])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"all"
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::validator]Validator::validate_required:iter:aog="Target"
[clap_builder::parser::validator]Validator::validate_required:iter: This is a group
[clap_builder::builder::command]Command::unroll_args_in_group: group="Target"
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "Target"
[clap_builder::parser::validator]Validator::missing_required_error; incl=["Target"]
[clap_builder::parser::validator]Validator::missing_required_error: reqs=ChildGraph([Child { id: "Target", children: [] }])
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=["Target"], matcher=true, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=["Target"]
[clap_builder::builder::command]Command::unroll_args_in_group: group="Target"
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"Target" group is_present=false
[clap_builder::builder::command]Command::unroll_args_in_group: group="Target"
[clap_builder::builder::command]Command::unroll_args_in_group: group="Target"
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"Target" group is_present=false
[clap_builder::builder::command]Command::unroll_args_in_group: group="Target"
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[StyledStr("<>")]
[clap_builder::parser::validator]Validator::missing_required_error: req_args=[
    "<>",
]
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_smart_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::write_args: incls=["all", "Target"]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["Target"]
[clap_builder::builder::command]Command::unroll_args_in_group: group="Target"
[clap_builder::builder::command]Command::unroll_args_in_group: group="Target"
[clap_builder::builder::command]Command::unroll_args_in_group: group="Target"
[clap_builder::builder::command]Command::unroll_args_in_group: group="Target"
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: binary test --all <>
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: the following required arguments were not provided:
  <>
```

---

_Label `C-bug` added by @nihaals on 2024-03-28 18:39_

---

_Comment by @epage on 2024-03-28 19:29_

I believe this is another variant of #4697.  There are ... issues when relying on the behavior of nested groups.

In both cases, when an argument is set, we also set the group.  However, that isn't recursive up the chain and so when the top-level group checks the status of its direct members, it gets incomplete information.

Closing in favor of #4697 though we should not this variant of the symptom.

If there is a reason we should keep this open separately, let us know!

---

_Closed by @epage on 2024-03-28 19:29_

---

_Referenced in [clap-rs/clap#4697](../../clap-rs/clap/issues/4697.md) on 2024-03-28 19:29_

---
