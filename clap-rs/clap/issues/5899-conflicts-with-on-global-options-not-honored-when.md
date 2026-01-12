```yaml
number: 5899
title: "`conflicts_with` on `global` options not honored when used across a subcommand"
type: issue
state: closed
author: jwodder
labels:
  - C-bug
assignees: []
created_at: 2025-02-03T12:51:07Z
updated_at: 2025-02-03T17:27:19Z
url: https://github.com/clap-rs/clap/issues/5899
synced_at: 2026-01-12T16:14:17Z
```

# `conflicts_with` on `global` options not honored when used across a subcommand

---

_@jwodder_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.84.1 (e71f9a9a9 2025-01-27)

### Clap Version

v4.5.27

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Clone, Copy, Debug, Eq, Parser, PartialEq)]
struct Arguments {
    #[arg(short, long, global = true)]
    quiet: bool,

    #[arg(short, long, global = true, conflicts_with = "quiet")]
    verbose: bool,

    #[command(subcommand)]
    command: Command,
}

#[derive(Clone, Copy, Debug, Eq, PartialEq, Subcommand)]
enum Command {
    List,
}

fn main() {
    println!("{:#?}", Arguments::parse());
}
```


### Steps to reproduce the bug with the above code

- `cargo add clap --features derive`
- `cargo run -- -q list -v`

### Actual Behaviour

```
Arguments {
    quiet: true,
    verbose: true,
    command: List,
}
```

### Expected Behaviour

```
error: the argument '--quiet' cannot be used with '--verbose'
```

### Additional Context

Clap correctly emits an error for `-q -v list` and `list -q -v`.

Possibly related issue: #1204

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-bug-20250203"
[clap_builder::builder::command]Command::_propagate:clap-bug-20250203
[clap_builder::builder::command]Command::_check_help_and_version:clap-bug-20250203 expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:clap-bug-20250203
[clap_builder::builder::command]Command::_propagate pushing "quiet" to list
[clap_builder::builder::command]Command::_propagate pushing "verbose" to list
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:quiet
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:verbose
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-q"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-q")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "q", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['q']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_arg:iter:q
[clap_builder::parser::parser]Parser::parse_short_arg:iter:q: Found valid opt or flag
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=Some(Short), source=CommandLine
[clap_builder::parser::parser]Parser::react: has default_missing_vals
[clap_builder::parser::parser]Parser::remove_overrides: id="quiet"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="quiet", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="quiet"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Arguments", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["true"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::get_matches_with: After parse_short_arg ValuesDone
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"list"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("list")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("list")
[clap_builder::parser::parser]Parser::parse_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of list to "clap-bug-20250203 list"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of list to "clap-bug-20250203-list"
[clap_builder::builder::command]Command::_build: name="list"
[clap_builder::builder::command]Command::_propagate:list
[clap_builder::builder::command]Command::_check_help_and_version:list expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:list
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:quiet
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:verbose
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=list
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-v"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-v")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "v", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['v']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_arg:iter:v
[clap_builder::parser::parser]Parser::parse_short_arg:iter:v: Found valid opt or flag
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=Some(Short), source=CommandLine
[clap_builder::parser::parser]Parser::react: has default_missing_vals
[clap_builder::parser::parser]Parser::remove_overrides: id="verbose"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="verbose", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="verbose"
[clap_builder::parser::parser]Parser::push_arg_values: ["true"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::get_matches_with: After parse_short_arg ValuesDone
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:quiet:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:quiet: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:quiet: wasn't used
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=None, source=DefaultValue
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="quiet", source=DefaultValue
[clap_builder::parser::parser]Parser::push_arg_values: ["false"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::parser]Parser::add_defaults:iter:verbose:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:verbose: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:verbose: was used
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="verbose"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="verbose", conflicts=["quiet"]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="verbose"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="verbose"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"verbose"
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:quiet:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:quiet: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:quiet: was used
[clap_builder::parser::parser]Parser::add_defaults:iter:verbose:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:verbose: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:verbose: wasn't used
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=None, source=DefaultValue
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="verbose", source=DefaultValue
[clap_builder::parser::parser]Parser::push_arg_values: ["false"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="quiet"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="quiet", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="Arguments", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="quiet"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="quiet"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"quiet"
[clap_builder::parser::validator]Validator::gather_requires:iter:"Arguments"
[clap_builder::parser::validator]Validator::gather_requires:iter:"Arguments":group
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::arg_matcher]ArgMatcher::get_global_values: global_arg_vec=["quiet", "verbose", "quiet", "verbose"]
Arguments {
    quiet: true,
    verbose: true,
    command: List,
}
```

---

_Label `C-bug` added by @jwodder on 2025-02-03 12:51_

---

_Comment by @epage on 2025-02-03 17:27_

We have a more general issue for this problem at #1546, closing in favor of that.

---

_Closed by @epage on 2025-02-03 17:27_

---
