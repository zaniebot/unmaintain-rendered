---
number: 6018
title: "`allow_hyphen_value` parsing does not recognize attached short argument"
type: issue
state: closed
author: euclio
labels:
  - C-bug
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2025-05-27T15:12:25Z
updated_at: 2025-05-27T17:00:47Z
url: https://github.com/clap-rs/clap/issues/6018
synced_at: 2026-01-07T13:12:20-06:00
---

# `allow_hyphen_value` parsing does not recognize attached short argument

---

_Issue opened by @euclio on 2025-05-27 15:12_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.87.0

### Clap Version

4.5.38

### Minimal reproducible code

```rust
let cmd = Command::new("build")
    .arg(arg!(-P --parallel <N>))
    .arg(arg!(<ARGS>...).trailing_var_arg(true).allow_hyphen_values(true));
    
let matches = cmd.clone().get_matches_from(vec!["build", "-P", "32", "foo", "--bar"]);

dbg!(&matches);

assert_eq!(matches.get_one("parallel"), Some(&String::from("32")));
let mut args = matches.get_raw("ARGS").unwrap().into_iter();
assert_eq!(args.next(), Some(OsStr::new("foo")));
assert_eq!(args.next(), Some(OsStr::new("--bar")));

let matches = cmd.get_matches_from(vec!["build", "-P32", "foo", "--bar"]);

dbg!(&matches);

assert_eq!(matches.get_one("parallel"), Some(&String::from("32")));
let mut args = matches.get_raw("ARGS").unwrap().into_iter();
assert_eq!(args.next(), Some(OsStr::new("foo")));
assert_eq!(args.next(), Some(OsStr::new("--bar")));
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

The second set of assertions fails, despite the only difference being that the argument is attached. `-P32` is parsed as the first vararg instead.

### Expected Behaviour

The second set of assertions should pass like the first set does.

### Additional Context

_No response_

### Debug Output

[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="build"
[clap_builder::builder::command]Command::_propagate:build
[clap_builder::builder::command]Command::_check_help_and_version:build expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:build
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:parallel
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:ARGS
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-P"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-P")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "P", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['P']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_arg:iter:P
[clap_builder::parser::parser]Parser::parse_short_arg:iter:P: Found valid opt or flag
[clap_builder::parser::parser]Parser::parse_short_arg:iter:P: val="", short_arg=ShortFlags { inner: "P", utf8_prefix: CharIndices { front_offset: 1, iter: Chars([]) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_opt_value; arg=parallel, val=None, has_eq=false
[clap_builder::parser::parser]Parser::parse_opt_value; arg.settings=ArgFlags(0)
[clap_builder::parser::parser]Parser::parse_opt_value; Checking for val...
[clap_builder::parser::parser]Parser::parse_opt_value: More arg vals required...
[clap_builder::parser::parser]Parser::get_matches_with: After parse_short_arg Opt("parallel")
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"32"'
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=parallel, pending=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=1
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"foo"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("foo")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]Parser::resolve_pending: id="parallel"
[clap_builder::parser::parser]Parser::react action=Set, identifier=Some(Short), source=CommandLine
[clap_builder::parser::parser]Parser::react: cur_idx:=1
[clap_builder::parser::parser]Parser::remove_overrides: id="parallel"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="parallel", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="parallel"
[clap_builder::parser::parser]Parser::push_arg_values: ["32"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=parallel, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--bar"'
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]Parser::resolve_pending: id="ARGS"
[clap_builder::parser::parser]Parser::react action=Append, identifier=Some(Index), source=CommandLine
[clap_builder::parser::parser]Parser::remove_overrides: id="ARGS"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="ARGS", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="ARGS"
[clap_builder::parser::parser]Parser::push_arg_values: ["foo", "--bar"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=3
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=4
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=ARGS, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1.., actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:parallel:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:parallel: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:ARGS:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:ARGS: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="parallel"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="parallel", conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="ARGS"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="ARGS", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"parallel"
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"ARGS"
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="parallel"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="parallel"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="ARGS"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="ARGS"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([Child { id: "ARGS", children: [] }])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"parallel"
[clap_builder::parser::validator]Validator::gather_requires:iter:"ARGS"
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::arg_matcher]ArgMatcher::get_global_values: global_arg_vec=[]
[src/main.rs:16:5] &matches = ArgMatches {
    valid_args: [
        "parallel",
        "ARGS",
        "help",
    ],
    valid_subcommands: [],
    args: FlatMap {
        keys: [
            "parallel",
            "ARGS",
        ],
        values: [
            MatchedArg {
                source: Some(
                    CommandLine,
                ),
                indices: [
                    2,
                ],
                type_id: Some(
                    alloc::string::String,
                ),
                vals: [
                    [
                        AnyValue {
                            inner: alloc::string::String,
                        },
                    ],
                ],
                raw_vals: [
                    [
                        "32",
                    ],
                ],
                ignore_case: false,
            },
            MatchedArg {
                source: Some(
                    CommandLine,
                ),
                indices: [
                    3,
                    4,
                ],
                type_id: Some(
                    alloc::string::String,
                ),
                vals: [
                    [
                        AnyValue {
                            inner: alloc::string::String,
                        },
                        AnyValue {
                            inner: alloc::string::String,
                        },
                    ],
                ],
                raw_vals: [
                    [
                        "foo",
                        "--bar",
                    ],
                ],
                ignore_case: false,
            },
        ],
    },
    subcommand: None,
}
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="build"
[clap_builder::builder::command]Command::_propagate:build
[clap_builder::builder::command]Command::_check_help_and_version:build expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:build
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:parallel
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:ARGS
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-P32"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-P32")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "P32", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['P', '3', '2']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_args: positional at 1 allows hyphens
[clap_builder::parser::parser]Parser::get_matches_with: After parse_short_arg MaybeHyphenValue
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"foo"'
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--bar"'
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]Parser::resolve_pending: id="ARGS"
[clap_builder::parser::parser]Parser::react action=Append, identifier=Some(Index), source=CommandLine
[clap_builder::parser::parser]Parser::remove_overrides: id="ARGS"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="ARGS", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="ARGS"
[clap_builder::parser::parser]Parser::push_arg_values: ["-P32", "foo", "--bar"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=3
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=ARGS, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1.., actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:parallel:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:parallel: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:ARGS:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:ARGS: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="ARGS"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="ARGS", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="ARGS"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="ARGS"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([Child { id: "ARGS", children: [] }])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"ARGS"
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::arg_matcher]ArgMatcher::get_global_values: global_arg_vec=[]
[src/main.rs:25:5] &matches = ArgMatches {
    valid_args: [
        "parallel",
        "ARGS",
        "help",
    ],
    valid_subcommands: [],
    args: FlatMap {
        keys: [
            "ARGS",
        ],
        values: [
            MatchedArg {
                source: Some(
                    CommandLine,
                ),
                indices: [
                    1,
                    2,
                    3,
                ],
                type_id: Some(
                    alloc::string::String,
                ),
                vals: [
                    [
                        AnyValue {
                            inner: alloc::string::String,
                        },
                        AnyValue {
                            inner: alloc::string::String,
                        },
                        AnyValue {
                            inner: alloc::string::String,
                        },
                    ],
                ],
                raw_vals: [
                    [
                        "-P32",
                        "foo",
                        "--bar",
                    ],
                ],
                ignore_case: false,
            },
        ],
    },
    subcommand: None,
}

---

_Label `C-bug` added by @euclio on 2025-05-27 15:12_

---

_Label `A-parsing` added by @epage on 2025-05-27 16:51_

---

_Label `S-waiting-on-design` added by @epage on 2025-05-27 16:51_

---

_Renamed from "Vararg parsing does not recognize attached short argument" to "`allow_hyphen_value` parsing does not recognize attached short argument" by @epage on 2025-05-27 16:54_

---

_Comment by @epage on 2025-05-27 16:59_

We are hitting the following code path
https://github.com/clap-rs/clap/blob/bd01bdc0ffa3308555a81ad170be9e02854b2e3f/clap_builder/src/parser/parser.rs#L901-L913

The behavior dates back to #4187 when `Arg::allow_hyphen_values` was made to behave like `Command::allow_hyphen_values`.

The problem is that we only check if all of the shorts are present.  We don't short circuit if a short takes a value.

Fixing this would change behavior but most likely in a way that people are assuming would work and it is unlikely this would break something.  I'm still a bit reticent to do this before clap v5.

---

_Comment by @epage on 2025-05-27 17:00_

Looking through our issues, I found #3410.  It looks to be the same and I'm tempted to close this in favor of that.  I'm unsure why we didn't entertain the thought of just checking if a short takes a value.

If there is something I missed and this should be left open on its own, let us know!

---

_Closed by @epage on 2025-05-27 17:00_

---

_Referenced in [clap-rs/clap#3410](../../clap-rs/clap/issues/3410.md) on 2025-05-27 17:01_

---
