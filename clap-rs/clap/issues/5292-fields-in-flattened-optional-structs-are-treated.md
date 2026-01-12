```yaml
number: 5292
title: Fields in flattened optional structs are treated as required
type: issue
state: closed
author: akonradi-signal
labels:
  - C-bug
assignees: []
created_at: 2024-01-08T20:20:29Z
updated_at: 2024-01-08T21:27:18Z
url: https://github.com/clap-rs/clap/issues/5292
synced_at: 2026-01-12T16:14:16Z
```

# Fields in flattened optional structs are treated as required

---

_@akonradi-signal_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.75.0

### Clap Version

4.4.14

### Minimal reproducible code

```rust
use clap::{Args, Parser};

#[derive(Debug, Parser)]
struct Cli {
    /// Arguments for main command
    #[command(flatten)]
    first: Option<FirstGroup>,
    
    #[command(flatten)]
    second: Option<SecondGroup>,
}

#[derive(Debug, Args)]
struct FirstGroup {
    #[arg(long)]
    first_a: bool,
    #[arg(long)]
    first_b: String,
}

#[derive(Debug, Args)]
struct SecondGroup {
    #[arg(long)]
    second_c: i32,
    #[arg(long)]
    second_d: bool
}

fn main() {
    Cli::parse_from(["playground"]);
}
```

([playground link](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=e82b6ad3dca30b44f9370189d14bb3b9))

### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

The program fails with
```
error: the following required arguments were not provided:
  --first-b <FIRST_B>
  --second-c <SECOND_C>
```

### Expected Behaviour

The program should succeed since all of the flags are (transitively) declared as optional.

### Additional Context

This might be the same issue as the discussion at https://github.com/clap-rs/clap/discussions/4954

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
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:first_a
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:first_b
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:second_c
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:second_d
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:first_a:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:first_a: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:first_a: wasn't used
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=None, source=DefaultValue
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="first_a", source=DefaultValue
[clap_builder::parser::parser]Parser::push_arg_values: ["false"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::add_defaults:iter:first_b:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:first_b: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:second_c:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:second_c: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:second_d:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:second_d: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:second_d: wasn't used
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=None, source=DefaultValue
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="second_d", source=DefaultValue
[clap_builder::parser::parser]Parser::push_arg_values: ["false"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([Child { id: "first_b", children: [] }, Child { id: "second_c", children: [] }])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::validator]Validator::validate_required:iter:aog="first_b"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::validator]Validator::is_missing_required_ok: first_b
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="first_b"
[clap_builder::builder::command]Command::groups_for_arg: id="first_b"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="first_b", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="first_b"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="FirstGroup"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="FirstGroup", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "first_b"
[clap_builder::parser::validator]Validator::validate_required:iter:aog="second_c"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::validator]Validator::is_missing_required_ok: second_c
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="second_c"
[clap_builder::builder::command]Command::groups_for_arg: id="second_c"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="second_c", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="second_c"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="SecondGroup"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="SecondGroup", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "second_c"
[clap_builder::parser::validator]Validator::missing_required_error; incl=["first_b", "second_c"]
[clap_builder::parser::validator]Validator::missing_required_error: reqs=ChildGraph([Child { id: "first_b", children: [] }, Child { id: "second_c", children: [] }])
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=["first_b", "second_c"], matcher=true, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=["first_b", "second_c"]
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"first_b" arg is_present=false
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"second_c" arg is_present=false
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"first_b" arg is_present=false
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"second_c" arg is_present=false
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[StyledStr("\u{1b}[1m--first-b\u{1b}[0m <FIRST_B>"), StyledStr("\u{1b}[1m--second-c\u{1b}[0m <SECOND_C>")]
[clap_builder::parser::validator]Validator::missing_required_error: req_args=[
    "--first-b <FIRST_B>",
    "--second-c <SECOND_C>",
]
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_smart_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::write_args: incls=["first_b", "second_c"]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["first_b", "second_c"]
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: playground --first-b <FIRST_B> --second-c <SECOND_C>
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: the following required arguments were not provided:
  --first-b <FIRST_B>
  --second-c <SECOND_C>

Usage: playground --first-b <FIRST_B> --second-c <SECOND_C>

For more information, try '--help'.
```

---

_Label `C-bug` added by @akonradi-signal on 2024-01-08 20:20_

---

_Comment by @epage on 2024-01-08 21:27_

This looks like a duplicate of #5092 so closing in favor of that.  If there is a reason for us to re-evaluate this, let us know!

---

_Closed by @epage on 2024-01-08 21:27_

---
