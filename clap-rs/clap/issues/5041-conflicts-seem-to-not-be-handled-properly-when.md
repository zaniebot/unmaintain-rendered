```yaml
number: 5041
title: Conflicts seem to not be handled properly when using a group
type: issue
state: open
author: rgiot
labels:
  - C-bug
  - E-medium
  - A-validators
assignees: []
created_at: 2023-07-22T16:00:31Z
updated_at: 2023-07-24T19:34:46Z
url: https://github.com/clap-rs/clap/issues/5041
synced_at: 2026-01-12T16:14:16Z
```

# Conflicts seem to not be handled properly when using a group

---

_@rgiot_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.73.0-nightly (4c8bb79d9 2023-07-15)

### Clap Version

4.3.19

### Minimal reproducible code

```rust
#!/usr/bin/env run-cargo-script


//! ```cargo
//! [dependencies]
//! clap = "4.3.19"
//! ```

extern crate clap;

fn main() {
    clap::Command::new("test")
        .disable_help_flag(true)
        .arg(clap::Arg::new("a").long("arg_a"))
        .arg(clap::Arg::new("b").value_hint(clap::ValueHint::FilePath))
        .arg(
            clap::Arg::new("c")
                .long("arg_c")
                .action(clap::ArgAction::SetTrue),
        )
        .group(
            clap::ArgGroup::new("g")
                .args(&["a", "b", "c"])
                .required(true),
        )
        .arg(
            clap::Arg::new("other")
                .long("other")
                .short('o')
                .action(clap::ArgAction::SetTrue)
                .conflicts_with_all(["a", "b", "c", "g"]),
        )
        .get_matches();
}

```


### Steps to reproduce the bug with the above code

 `run-cargo-script.exe .\clap1.rs  --other`


### Actual Behaviour

The following error is obtained:

```
error: the following required arguments were not provided:
  <--arg_a <a>|b|--arg_c>

Usage: clap1.exe --other <--arg_a <a>|b|--arg_c>
```


### Expected Behaviour


As other is set to conflict with a, b, c, and even g, there should be no problem.

I would say that the behavior should be similar to this one:

```rust
#!/usr/bin/env run-cargo-script



//! ```cargo
//! [dependencies]
//! clap = "4.3.19"
//! ```

extern crate clap;

fn main() {
    clap::Command::new("test")
        .disable_help_flag(true)
        .arg(clap::Arg::new("a").long("arg_a").required(true))
        .arg(
            clap::Arg::new("b")
                .value_hint(clap::ValueHint::FilePath)
                .required(true),
        )
        .arg(
            clap::Arg::new("c")
                .long("arg_c")
                .action(clap::ArgAction::SetTrue)
                .required(true),
        )
        .arg(
            clap::Arg::new("other")
                .long("other")
                .short('o')
                .action(clap::ArgAction::SetTrue)
                .conflicts_with_all(["a", "b", "c"]),
        )
        .get_matches();
}
```


`run-cargo-script.exe .\clap2.rs  --other` raises no issue


### Additional Context

Maybe it is related to #4520 or #4707, or maybe I misunderstood some details in clap way of working

### Debug Output

Output of clap1

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="test"
[clap_builder::builder::command]Command::_propagate:test
[clap_builder::builder::command]Command::_check_help_and_version:test expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_propagate_global_args:test
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--other"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--other")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--other'
[clap_builder::parser::parser]Parser::parse_long_arg("other"): Presence validated
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Parser::react: has default_missing_vals
[clap_builder::parser::parser]Parser::remove_overrides: id="other"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="other", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="other"
[clap_builder::parser::parser]Parser::push_arg_values: ["true"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::get_matches_with: After parse_long_arg ValuesDone
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:a:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:a: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:b:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:b: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:c:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:c: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:c: wasn't used
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=None, source=DefaultValue
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="c", source=DefaultValue
[clap_builder::parser::parser]Parser::push_arg_values: ["false"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::parser]Parser::add_defaults:iter:other:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:other: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:other: was used
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="other"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="other", conflicts=["a", "b", "c", "g"]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="other"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="other"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([Child { id: "g", children: [] }])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"other"
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::validator]Validator::validate_required:iter:aog="g"
[clap_builder::parser::validator]Validator::validate_required:iter: This is a group
[clap_builder::builder::command]Command::unroll_args_in_group: group="g"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="a"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="b"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="c"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "g"
[clap_builder::parser::validator]Validator::missing_required_error; incl=["g"]
[clap_builder::parser::validator]Validator::missing_required_error: reqs=ChildGraph([Child { id: "g", children: [] }])
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=["g"], matcher=true, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=["g"]
[clap_builder::builder::command]Command::unroll_args_in_group: group="g"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="a"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="b"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="c"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"g" group is_present=false
[clap_builder::builder::command]Command::unroll_args_in_group: group="g"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="a"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="b"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="c"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[  clap_builder::builder::arg]Arg::name_no_brackets:b
[  clap_builder::builder::arg]Arg::name_no_brackets: just name
[clap_builder::builder::command]Command::unroll_args_in_group: group="g"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="a"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="b"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="c"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"g" group is_present=false
[clap_builder::builder::command]Command::unroll_args_in_group: group="g"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="a"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="b"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="c"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[  clap_builder::builder::arg]Arg::name_no_brackets:b
[  clap_builder::builder::arg]Arg::name_no_brackets: just name
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[StyledStr("<--arg_a <a>|b|--arg_c>")]
[clap_builder::parser::validator]Validator::missing_required_error: req_args=[
    "<--arg_a <a>|b|--arg_c>",
]
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_smart_usage
[ clap_builder::output::usage]Usage::get_args: incls=["other", "g"]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["g"]
[clap_builder::builder::command]Command::unroll_args_in_group: group="g"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="a"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="b"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="c"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group: group="g"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="a"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="b"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="c"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[  clap_builder::builder::arg]Arg::name_no_brackets:b
[  clap_builder::builder::arg]Arg::name_no_brackets: just name
[clap_builder::builder::command]Command::unroll_args_in_group: group="g"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="a"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="b"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="c"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group: group="g"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="a"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="b"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="c"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[  clap_builder::builder::arg]Arg::name_no_brackets:b
[  clap_builder::builder::arg]Arg::name_no_brackets: just name
[ clap_builder::output::usage]Usage::get_args: ret_val=[StyledStr("\u{1b}[1m--other\u{1b}[0m"), StyledStr("<--arg_a <a>|b|--arg_c>")]
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: the following required arguments were not provided:
  <--arg_a <a>|b|--arg_c>
```


Output of clap2
```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="test"
[clap_builder::builder::command]Command::_propagate:test
[clap_builder::builder::command]Command::_check_help_and_version:test expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_propagate_global_args:test
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--other"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--other")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--other'
[clap_builder::parser::parser]Parser::parse_long_arg("other"): Presence validated
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Parser::react: has default_missing_vals
[clap_builder::parser::parser]Parser::remove_overrides: id="other"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="other", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="other"
[clap_builder::parser::parser]Parser::push_arg_values: ["true"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::get_matches_with: After parse_long_arg ValuesDone
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:a:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:a: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:b:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:b: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:c:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:c: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:c: wasn't used
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=None, source=DefaultValue
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="c", source=DefaultValue
[clap_builder::parser::parser]Parser::push_arg_values: ["false"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::parser]Parser::add_defaults:iter:other:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:other: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:other: was used
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="other"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="other", conflicts=["a", "b", "c"]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="other"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="other"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([Child { id: "a", children: [] }, Child { id: "b", children: [] }, Child { id: "c", children: [] }])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"other"
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::validator]Validator::validate_required:iter:aog="a"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::validator]Validator::is_missing_required_ok: a
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="a"
[clap_builder::builder::command]Command::groups_for_arg: id="a"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="a", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=["other"]
[clap_builder::parser::validator]Validator::is_missing_required_ok: true (self)
[clap_builder::parser::validator]Validator::validate_required:iter:aog="b"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::validator]Validator::is_missing_required_ok: b
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="b"
[clap_builder::builder::command]Command::groups_for_arg: id="b"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="b", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=["other"]
[clap_builder::parser::validator]Validator::is_missing_required_ok: true (self)
[clap_builder::parser::validator]Validator::validate_required:iter:aog="c"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::validator]Validator::is_missing_required_ok: c
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="c"
[clap_builder::builder::command]Command::groups_for_arg: id="c"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="c", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=["other"]
[clap_builder::parser::validator]Validator::is_missing_required_ok: true (self)
[clap_builder::parser::arg_matcher]ArgMatcher::get_global_values: global_arg_vec=[]
```

---

_Label `C-bug` added by @rgiot on 2023-07-22 16:00_

---

_Referenced in [clap-rs/clap#5038](../../clap-rs/clap/issues/5038.md) on 2023-07-22 16:01_

---

_Label `E-medium` added by @epage on 2023-07-24 19:34_

---

_Label `A-validators` added by @epage on 2023-07-24 19:34_

---

_Referenced in [clap-rs/clap#5253](../../clap-rs/clap/issues/5253.md) on 2023-12-11 16:44_

---

_Referenced in [astral-sh/uv#3829](../../astral-sh/uv/issues/3829.md) on 2024-05-24 18:47_

---
