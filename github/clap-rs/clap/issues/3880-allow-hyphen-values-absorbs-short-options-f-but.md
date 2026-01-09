---
number: 3880
title: "\"allow_hyphen_values\" absorbs short options (-f), but not long options (--flag)"
type: issue
state: closed
author: david0u0
labels:
  - C-bug
  - M-breaking-change
  - A-parsing
assignees: []
created_at: 2022-06-28T09:14:55Z
updated_at: 2022-09-07T12:39:26Z
url: https://github.com/clap-rs/clap/issues/3880
synced_at: 2026-01-07T13:12:20-06:00
---

# "allow_hyphen_values" absorbs short options (-f), but not long options (--flag)

---

_Issue opened by @david0u0 on 2022-06-28 09:14_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.60.0-nightly (88fb06a1f 2022-02-05)

### Clap Version

3.1.8

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser, Debug)]
#[clap(disable_help_subcommand = true, args_override_self = true)]
pub enum Root {
    #[clap(allow_hyphen_values = true)]
    Run {
        #[clap(long, short)]
        flag: bool,
        #[clap(default_value = "-")]
        query: String,
        #[clap(allow_hyphen_values = true)]
        args: Vec<String>,
    },
}

fn handle_args(s: &str) -> Root {
    Root::from_iter(s.split(' '))
}

fn extract(r: Root) -> Vec<String> {
    match r {
        Root::Run {args, ..} => args
    }
}

fn main() {
    let root = handle_args("test run query -f");
    println!("{:?}", root);
    assert_eq!(extract(root), vec!["-f"]);

    let root = handle_args("test run query --flag");
    println!("{:?}", root);
    assert_eq!(extract(root), vec!["--flag"]); // <- fail!!
}
```

### Steps to reproduce the bug with the above code

`cargo run`

The second assert will fail

### Actual Behaviour

With argument "test run query --flag", the "--flag" will be treated as the option, instead of the positional argument

This seems be not consistent, because the short option "-f" will be treated as the positional argument

### Expected Behaviour

Treat the long option as positional argument

Or treat the short option as option?

### Additional Context

_No response_

### Debug Output

# first
[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="clap-hyphen"
[      clap::builder::command] 	Command::_propagate:clap-hyphen
[      clap::builder::command] 	Command::_check_help_and_version: clap-hyphen
[      clap::builder::command] 	Command::_check_help_and_version: Removing generated version
[      clap::builder::command] 	Command::_propagate_global_args:clap-hyphen
[      clap::builder::command] 	Command::_propagate removing run's help
[      clap::builder::command] 	Command::_propagate pushing help to run
[      clap::builder::command] 	Command::_derive_display_order:clap-hyphen
[      clap::builder::command] 	Command::_derive_display_order:run
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("run")' ([114, 117, 110])
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...1
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("run")
[        clap::parser::parser] 	Parser::get_matches_with: sc=Some("run")
[        clap::parser::parser] 	Parser::parse_subcommand
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val={}
[      clap::builder::command] 	Command::_build_subcommand Setting bin_name of run to "test run"
[      clap::builder::command] 	Command::_build_subcommand Setting display_name of run to "clap-hyphen-run"
[      clap::builder::command] 	Command::_build: name="run"
[      clap::builder::command] 	Command::_propagate:run
[      clap::builder::command] 	Command::_check_help_and_version: run
[      clap::builder::command] 	Command::_check_help_and_version: Removing generated version
[      clap::builder::command] 	Command::_propagate_global_args:run
[      clap::builder::command] 	Command::_derive_display_order:run
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:flag
[clap::builder::debug_asserts] 	Arg::_debug_asserts:query
[clap::builder::debug_asserts] 	Arg::_debug_asserts:args
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::parse_subcommand: About to parse sc=run
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("query")' ([113, 117, 101, 114, 121])
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...1
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("query")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::split_arg_values; arg=query, val=RawOsStr("query")
[        clap::parser::parser] 	Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("-f")' ([45, 102])
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...2
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("-f")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("f"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['f']) }, invalid_suffix: None }
[        clap::parser::parser] 	Parser::parse_short_args: positional at 2 allows hyphens
[        clap::parser::parser] 	Parser::get_matches_with: After parse_short_arg MaybeHyphenValue
[        clap::parser::parser] 	Parser::resolve_pending: id=query
[        clap::parser::parser] 	Parser::react action=StoreValue, identifier=Some(Index), source=CommandLine
[        clap::parser::parser] 	Parser::remove_overrides: id=query
[   clap::parser::arg_matcher] 	ArgMatcher::start_occurrence_of_arg: id=query
[      clap::builder::command] 	Command::groups_for_arg: id=query
[        clap::parser::parser] 	Parser::push_arg_values: ["query"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[      clap::builder::command] 	Command::groups_for_arg: id=query
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=query, resolved=1, pending=0
[        clap::parser::parser] 	Parser::split_arg_values; arg=args, val=RawOsStr("-f")
[        clap::parser::parser] 	Parser::split_arg_values; trailing_values=true, DontDelimTrailingVals=false
[        clap::parser::parser] 	Parser::resolve_pending: id=args
[        clap::parser::parser] 	Parser::react action=StoreValue, identifier=Some(Index), source=CommandLine
[        clap::parser::parser] 	Parser::remove_overrides: id=args
[   clap::parser::arg_matcher] 	ArgMatcher::start_occurrence_of_arg: id=args
[      clap::builder::command] 	Command::groups_for_arg: id=args
[        clap::parser::parser] 	Parser::push_arg_values: ["-f"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=2
[      clap::builder::command] 	Command::groups_for_arg: id=args
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=args, resolved=1, pending=0
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:flag:
[        clap::parser::parser] 	Parser::add_default_value:iter:flag: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:flag: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:query:
[        clap::parser::parser] 	Parser::add_default_value:iter:query: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:query: has default vals
[        clap::parser::parser] 	Parser::add_default_value:iter:query: was used
[        clap::parser::parser] 	Parser::add_defaults:iter:args:
[        clap::parser::parser] 	Parser::add_default_value:iter:args: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:args: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[     clap::parser::validator] 	Validator::validate
[     clap::parser::validator] 	Validator::validate_conflicts
[     clap::parser::validator] 	Validator::validate_exclusive
[     clap::parser::validator] 	Validator::validate_exclusive:iter:query
[     clap::parser::validator] 	Validator::validate_exclusive:iter:args
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id=query
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg=query
[      clap::builder::command] 	Command::groups_for_arg: id=query
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id=query, conflicts=[]
[      clap::builder::command] 	Command::groups_for_arg: id=args
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id=args, conflicts=[]
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id=args
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg=args
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator] 	Validator::gather_requires
[     clap::parser::validator] 	Validator::gather_requires:iter:query
[     clap::parser::validator] 	Validator::gather_requires:iter:args
[     clap::parser::validator] 	Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator] 	Validator::validate_required_unless
[     clap::parser::validator] 	Validator::validate_matched_args
[     clap::parser::validator] 	Validator::validate_matched_args:iter:query: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            AnyValue {
                                inner: alloc::string::String,
                            },
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[     clap::parser::validator] 	Validator::validate_arg_num_vals
[     clap::parser::validator] 	Validator::validate_arg_values: arg="query"
[     clap::parser::validator] 	Validator::validate_arg_values: checking validator...
[     clap::parser::validator] 	good
[     clap::parser::validator] 	Validator::validate_arg_num_occurs: "query"=1
[     clap::parser::validator] 	Validator::validate_matched_args:iter:args: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            AnyValue {
                                inner: alloc::string::String,
                            },
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[     clap::parser::validator] 	Validator::validate_arg_num_vals
[     clap::parser::validator] 	Validator::validate_arg_values: arg="args"
[     clap::parser::validator] 	Validator::validate_arg_values: checking validator...
[     clap::parser::validator] 	good
[     clap::parser::validator] 	Validator::validate_arg_num_occurs: "args"=1
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[     clap::parser::validator] 	Validator::validate
[     clap::parser::validator] 	Validator::validate_conflicts
[     clap::parser::validator] 	Validator::validate_exclusive
[     clap::parser::validator] 	Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator] 	Validator::gather_requires
[     clap::parser::validator] 	Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator] 	Validator::validate_required_unless
[     clap::parser::validator] 	Validator::validate_matched_args
[   clap::parser::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[help, help]


# second

[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="clap-hyphen"
[      clap::builder::command] 	Command::_propagate:clap-hyphen
[      clap::builder::command] 	Command::_check_help_and_version: clap-hyphen
[      clap::builder::command] 	Command::_check_help_and_version: Removing generated version
[      clap::builder::command] 	Command::_propagate_global_args:clap-hyphen
[      clap::builder::command] 	Command::_propagate removing run's help
[      clap::builder::command] 	Command::_propagate pushing help to run
[      clap::builder::command] 	Command::_derive_display_order:clap-hyphen
[      clap::builder::command] 	Command::_derive_display_order:run
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("run")' ([114, 117, 110])
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...1
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("run")
[        clap::parser::parser] 	Parser::get_matches_with: sc=Some("run")
[        clap::parser::parser] 	Parser::parse_subcommand
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val={}
[      clap::builder::command] 	Command::_build_subcommand Setting bin_name of run to "test run"
[      clap::builder::command] 	Command::_build_subcommand Setting display_name of run to "clap-hyphen-run"
[      clap::builder::command] 	Command::_build: name="run"
[      clap::builder::command] 	Command::_propagate:run
[      clap::builder::command] 	Command::_check_help_and_version: run
[      clap::builder::command] 	Command::_check_help_and_version: Removing generated version
[      clap::builder::command] 	Command::_propagate_global_args:run
[      clap::builder::command] 	Command::_derive_display_order:run
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:flag
[clap::builder::debug_asserts] 	Arg::_debug_asserts:query
[clap::builder::debug_asserts] 	Arg::_debug_asserts:args
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::parse_subcommand: About to parse sc=run
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("query")' ([113, 117, 101, 114, 121])
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...1
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("query")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::split_arg_values; arg=query, val=RawOsStr("query")
[        clap::parser::parser] 	Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--flag")' ([45, 45, 102, 108, 97, 103])
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...2
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("--flag")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::parse_long_arg
[        clap::parser::parser] 	Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser] 	Parser::parse_long_arg: Found valid arg or flag '--flag'
[        clap::parser::parser] 	Parser::parse_long_arg("flag"): Presence validated
[        clap::parser::parser] 	Parser::resolve_pending: id=query
[        clap::parser::parser] 	Parser::react action=StoreValue, identifier=Some(Index), source=CommandLine
[        clap::parser::parser] 	Parser::remove_overrides: id=query
[   clap::parser::arg_matcher] 	ArgMatcher::start_occurrence_of_arg: id=query
[      clap::builder::command] 	Command::groups_for_arg: id=query
[        clap::parser::parser] 	Parser::push_arg_values: ["query"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[      clap::builder::command] 	Command::groups_for_arg: id=query
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=query, resolved=1, pending=0
[        clap::parser::parser] 	Parser::react action=IncOccurrence, identifier=Some(Long), source=CommandLine
[        clap::parser::parser] 	Parser::react: cur_idx:=2
[        clap::parser::parser] 	Parser::remove_overrides: id=flag
[        clap::parser::parser] 	Parser::remove_overrides:iter:flag: removing
[   clap::parser::arg_matcher] 	ArgMatcher::start_occurrence_of_arg: id=flag
[      clap::builder::command] 	Command::groups_for_arg: id=flag
[        clap::parser::parser] 	Parser::get_matches_with: After parse_long_arg ValuesDone
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:flag:
[        clap::parser::parser] 	Parser::add_default_value:iter:flag: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:flag: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:query:
[        clap::parser::parser] 	Parser::add_default_value:iter:query: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:query: has default vals
[        clap::parser::parser] 	Parser::add_default_value:iter:query: was used
[        clap::parser::parser] 	Parser::add_defaults:iter:args:
[        clap::parser::parser] 	Parser::add_default_value:iter:args: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:args: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[     clap::parser::validator] 	Validator::validate
[     clap::parser::validator] 	Validator::validate_conflicts
[     clap::parser::validator] 	Validator::validate_exclusive
[     clap::parser::validator] 	Validator::validate_exclusive:iter:query
[     clap::parser::validator] 	Validator::validate_exclusive:iter:flag
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id=query
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg=query
[      clap::builder::command] 	Command::groups_for_arg: id=query
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id=query, conflicts=[]
[      clap::builder::command] 	Command::groups_for_arg: id=flag
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id=flag, conflicts=[flag]
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id=flag
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg=flag
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator] 	Validator::gather_requires
[     clap::parser::validator] 	Validator::gather_requires:iter:query
[     clap::parser::validator] 	Validator::gather_requires:iter:flag
[     clap::parser::validator] 	Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator] 	Validator::validate_required_unless
[     clap::parser::validator] 	Validator::validate_matched_args
[     clap::parser::validator] 	Validator::validate_matched_args:iter:query: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            AnyValue {
                                inner: alloc::string::String,
                            },
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[     clap::parser::validator] 	Validator::validate_arg_num_vals
[     clap::parser::validator] 	Validator::validate_arg_values: arg="query"
[     clap::parser::validator] 	Validator::validate_arg_values: checking validator...
[     clap::parser::validator] 	good
[     clap::parser::validator] 	Validator::validate_arg_num_occurs: "query"=1
[     clap::parser::validator] 	Validator::validate_matched_args:iter:flag: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[     clap::parser::validator] 	Validator::validate_arg_num_vals
[     clap::parser::validator] 	Validator::validate_arg_values: arg="flag"
[     clap::parser::validator] 	Validator::validate_arg_num_occurs: "flag"=1
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[     clap::parser::validator] 	Validator::validate
[     clap::parser::validator] 	Validator::validate_conflicts
[     clap::parser::validator] 	Validator::validate_exclusive
[     clap::parser::validator] 	Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator] 	Validator::gather_requires
[     clap::parser::validator] 	Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator] 	Validator::validate_required_unless
[     clap::parser::validator] 	Validator::validate_matched_args
[   clap::parser::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[help, help]

---

_Label `C-bug` added by @david0u0 on 2022-06-28 09:14_

---

_Referenced in [clap-rs/clap#3887](../../clap-rs/clap/issues/3887.md) on 2022-06-30 04:09_

---

_Referenced in [clap-rs/clap#4039](../../clap-rs/clap/pulls/4039.md) on 2022-08-08 02:52_

---

_Comment by @tedinski on 2022-08-19 21:19_

I ran into this as well. I expected short and long args to be treated identically w.r.t. `allow_hyphen_values`, instead if you have "allow_hyphen_values" enabled as a final positional argument, clap will recognize long options before it but not short options.

```rust
fn main() {
    let config = CompilerArgs::parse();
    println!("{:?}", config);
}

use anyhow::Result;
use clap::Parser;

#[derive(Debug, Parser)]
#[clap(name = "clap-bug", allow_hyphen_values(true))]
pub struct CompilerArgs {
    /// a flag
    #[clap(long, short = 'O')]
    pub other: bool,

    /// Eat the rest
    #[clap(allow_hyphen_values = true, min_values(0))]
    pub remainder: Vec<String>,
}

fn test_case(args: &[&str]) -> Result<()> {
    println!("Beginning test case {:?}", args);
    let config = CompilerArgs::try_parse_from(args)?;
    println!("Got {:?}", config);
    assert!(config.other);
    Ok(())
}

#[test]
fn check_just_long() -> Result<()> {
    test_case(&["clap-bug", "--other"])
    // always works
}
#[test]
fn check_just_short() -> Result<()> {
    test_case(&["clap-bug", "-O"])
    // always fails: other = false, remainder = ["-O"]
    // expected: other = true, remainder = []
}
#[test]
fn check_short() -> Result<()> {
    test_case(&["clap-bug", "-O", "--extra"])
    // always fails: other = false, remainder = ["-O", "--extra"]
    // expected: other = true, remainder = ["--extra"]
}
#[test]
fn check_long() -> Result<()> {
    test_case(&["clap-bug", "--other", "--extra"])
    // Without AppSettings::AllowHyphenValues fails:
    // error: Found argument '--extra' which wasn't expected, or isn't valid in this context
    // With AppSettings::AllowHyphenValues, works. This seems odd.
    // I expected it to work without, and probably don't want to globally enable that option.
}
#[test]
fn check_long_delimiter() -> Result<()> {
    test_case(&["clap-bug", "--other", "--", "--extra"])
    // always works
}
```

Is there any way to get this behavior from clap 3.x? (That is, greedily parsing short options too, not just long, and only switch to collecting the remainder at an unrecognized option.) This is reduced from a failed attempt to upgrade from 2.x.

(I thought we might be able to switch to requiring `--`, but it turns out this will break things, not an option.)

---

_Label `M-breaking-change` added by @epage on 2022-08-26 20:59_

---

_Label `A-parsing` added by @epage on 2022-08-26 20:59_

---

_Added to milestone `4.0` by @epage on 2022-08-26 21:00_

---

_Comment by @epage on 2022-08-26 21:00_

At this point, we've probably had the behavior long enough that I would be hesitant to change it during 3.0.  I am getting close on 4.0.0 being ready and already need to look at some `allow_hyphen_value` behavior, so I can look at this as part of that

---

_Referenced in [clap-rs/clap#4187](../../clap-rs/clap/pulls/4187.md) on 2022-09-07 02:28_

---

_Closed by @epage on 2022-09-07 12:39_

---

_Referenced in [model-checking/kani#1647](../../model-checking/kani/issues/1647.md) on 2022-09-07 18:24_

---
