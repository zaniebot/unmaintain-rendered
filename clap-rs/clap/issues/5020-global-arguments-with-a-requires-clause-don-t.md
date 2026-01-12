```yaml
number: 5020
title: "Global arguments with a `requires` clause don't work under subcommands when specified through environment variables"
type: issue
state: open
author: Xophmeister
labels:
  - C-bug
  - A-parsing
  - A-validators
assignees: []
created_at: 2023-07-19T13:25:18Z
updated_at: 2025-11-13T13:59:22Z
url: https://github.com/clap-rs/clap/issues/5020
synced_at: 2026-01-12T16:14:16Z
```

# Global arguments with a `requires` clause don't work under subcommands when specified through environment variables

---

_@Xophmeister_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.70.0 (90c541806 2023-05-31) (built from a source tarball)

### Clap Version

4.3.13

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Parser, Debug)]
struct TestCli {
    #[arg(short, env = "FOO", global = true)]
    foo: Option<String>,

    #[arg(short, env = "BAR", requires = "foo", global = true)]
    bar: Option<String>,

    #[command(subcommand)]
    command: Commands,
}

#[derive(Debug, Subcommand)]
enum Commands {
    Quux,
}

fn main() {
    let cli = TestCli::parse();
    println!("{:?}", cli);
}
```

### Steps to reproduce the bug with the above code

```
BAR=123 cargo run -- quux -f abc
```

### Actual Behaviour

Complains about the `-f FOO` argument being missing, even though it isn't:

```
error: the following required arguments were not provided:
  -f <FOO>

Usage: example quux -f <FOO> -b <BAR>
```

### Expected Behaviour

Outputs the following:

```
TestCli { global: GlobalArgs { foo: Some("abc"), bar: Some("123") }, command: Quux }
```

### Additional Context

This _only_ appears to happen within the subcommand. If the subcommands are optional, this works as expected (but only at the top-level, when no subcommand is specified):

```
BAR=123 cargo run -- -f abc
```

With non-optional subcommands, it does work as expected if the required argument is also specified through its environment variable:

```
FOO=abc BAR=123 cargo run -- quux
```

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="example"
[clap_builder::builder::command]Command::_propagate:example
[clap_builder::builder::command]Command::_check_help_and_version:example expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:example
[clap_builder::builder::command]Command::_propagate pushing "foo" to quux
[clap_builder::builder::command]Command::_propagate pushing "bar" to quux
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:foo
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:bar
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"quux"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("quux")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("quux")
[clap_builder::parser::parser]Parser::parse_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of quux to "example quux"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of quux to "example-quux"
[clap_builder::builder::command]Command::_build: name="quux"
[clap_builder::builder::command]Command::_propagate:quux
[clap_builder::builder::command]Command::_check_help_and_version:quux expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:quux
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:foo
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:bar
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=quux
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-f"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-f")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "f", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['f']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_arg:iter:f
[clap_builder::parser::parser]Parser::parse_short_arg:iter:f: Found valid opt or flag
[clap_builder::parser::parser]Parser::parse_short_arg:iter:f: val="", short_arg=ShortFlags { inner: "f", utf8_prefix: CharIndices { front_offset: 1, iter: Chars([]) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_opt_value; arg=foo, val=None, has_eq=false
[clap_builder::parser::parser]Parser::parse_opt_value; arg.settings=ArgFlags(2)
[clap_builder::parser::parser]Parser::parse_opt_value; Checking for val...
[clap_builder::parser::parser]Parser::parse_opt_value: More arg vals required...
[clap_builder::parser::parser]Parser::get_matches_with: After parse_short_arg Opt("foo")
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"x"'
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=foo, pending=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=1
[clap_builder::parser::parser]Parser::resolve_pending: id="foo"
[clap_builder::parser::parser]Parser::react action=Set, identifier=Some(Short), source=CommandLine
[clap_builder::parser::parser]Parser::react: cur_idx:=1
[clap_builder::parser::parser]Parser::remove_overrides: id="foo"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="foo", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="foo"
[clap_builder::parser::parser]Parser::push_arg_values: ["x"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=foo, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::add_env
[clap_builder::parser::parser]Parser::add_env: Skipping existing arg `-f <FOO>`
[clap_builder::parser::parser]Parser::add_env: Checking arg `-b <BAR>`
[clap_builder::parser::parser]Parser::add_env: Found an opt with value="123"
[clap_builder::parser::parser]Parser::react action=Set, identifier=None, source=EnvVariable
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="bar", source=EnvVariable
[clap_builder::builder::command]Command::groups_for_arg: id="bar"
[clap_builder::parser::parser]Parser::push_arg_values: ["123"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=3
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=bar, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::add_env: Checking arg `--help`
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:foo:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:foo: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:bar:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:bar: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="foo"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="foo", conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="bar"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="bar", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"foo"
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"bar"
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="foo"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="foo"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="bar"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="bar"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"foo"
[clap_builder::parser::validator]Validator::gather_requires:iter:"bar"
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::parser]Parser::add_env
[clap_builder::parser::parser]Parser::add_env: Checking arg `-f <FOO>`
[clap_builder::parser::parser]Parser::add_env: Checking arg `-b <BAR>`
[clap_builder::parser::parser]Parser::add_env: Found an opt with value="123"
[clap_builder::parser::parser]Parser::react action=Set, identifier=None, source=EnvVariable
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="bar", source=EnvVariable
[clap_builder::builder::command]Command::groups_for_arg: id="bar"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="GlobalArgs", source=EnvVariable
[clap_builder::parser::parser]Parser::push_arg_values: ["123"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=bar, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::add_env: Checking arg `--help`
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:foo:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:foo: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:bar:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:bar: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="bar"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="bar", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="GlobalArgs", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="bar"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="bar"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"bar"
[clap_builder::parser::validator]Validator::gather_requires:iter:"GlobalArgs"
[clap_builder::parser::validator]Validator::gather_requires:iter:"GlobalArgs":group
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::validator]Validator::validate_required:iter:aog="foo"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::validator]Validator::is_missing_required_ok: foo
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="foo"
[clap_builder::builder::command]Command::groups_for_arg: id="foo"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="foo", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="foo"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="GlobalArgs"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "foo"
[clap_builder::parser::validator]Validator::missing_required_error; incl=["foo"]
[clap_builder::parser::validator]Validator::missing_required_error: reqs=ChildGraph([Child { id: "foo", children: [] }])
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=["foo"], matcher=true, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=["foo"]
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"foo" arg is_present=false
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"foo" arg is_present=false
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[StyledStr("\u{1b}[1m-f\u{1b}[0m <FOO>")]
[clap_builder::parser::validator]Validator::missing_required_error: req_args=[
    "-f <FOO>",
]
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_smart_usage
[ clap_builder::output::usage]Usage::get_args: incls=["bar", "foo"]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["foo"]
[ clap_builder::output::usage]Usage::get_args: ret_val=[StyledStr("\u{1b}[1m-f\u{1b}[0m <FOO>"), StyledStr("\u{1b}[1m-b\u{1b}[0m <BAR>")]
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
```

---

_Label `C-bug` added by @Xophmeister on 2023-07-19 13:25_

---

_Comment by @epage on 2023-07-19 13:55_

This is tied very closely to #1546 except we aren't verifying other required-related fields.

---

_Referenced in [EdJoPaTo/mqttui#169](../../EdJoPaTo/mqttui/issues/169.md) on 2024-05-29 20:20_

---

_Referenced in [clap-rs/clap#5588](../../clap-rs/clap/issues/5588.md) on 2024-07-19 16:06_

---

_Label `A-parsing` added by @epage on 2025-04-24 14:16_

---

_Label `A-validators` added by @epage on 2025-04-24 14:16_

---

_Referenced in [clap-rs/clap#5977](../../clap-rs/clap/issues/5977.md) on 2025-04-24 14:16_

---

_Referenced in [clap-rs/clap#6049](../../clap-rs/clap/issues/6049.md) on 2025-06-27 15:16_

---

_Label `:money_with_wings: $20` added by @pksunkara on 2025-11-13 13:58_

---

_Label `:money_with_wings: $20` removed by @pksunkara on 2025-11-13 13:59_

---
