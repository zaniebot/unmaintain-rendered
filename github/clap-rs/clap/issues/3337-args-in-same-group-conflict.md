---
number: 3337
title: Args in same group conflict 
type: issue
state: closed
author: nissaofthesea
labels:
  - C-bug
assignees: []
created_at: 2022-01-25T06:22:49Z
updated_at: 2022-01-25T13:21:14Z
url: https://github.com/clap-rs/clap/issues/3337
synced_at: 2026-01-07T13:12:19-06:00
---

# Args in same group conflict 

---

_Issue opened by @nissaofthesea on 2022-01-25 06:22_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.58.1 (db9d1b20b 2022-01-20)

### Clap Version

3.0.12

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Debug, Parser)]
struct Args {
    /// Sets the USB device to use
    #[clap(
        short = 'd',
        required = true,
        group = "usb"
    )]
    device: Option<u16>,

    /// Sets the time to wait for USB reads and writes
    #[clap(
        short = 't',
        required = true,
        group = "usb"
    )]
    timeout: Option<u64>,
}

fn main() {
    dbg!(Args::parse());
}
```


### Steps to reproduce the bug with the above code

```console
$ cargo run -- -d 123 -t 123
# output from clap "debug" feature is omitted
error: The argument '-d <DEVICE>' cannot be used with '-t <TIMEOUT>'

USAGE:
    program-name -d <DEVICE>

For more information try --help
```


### Actual Behaviour

`device` and `timeout` don't conflict with each other, yet they cannot both be present. Also, if one of them is missing, clap gives an error saying that the other was not provided.

### Expected Behaviour

`device` and `timeout` should be able to both be present, since they don't conflict.

### Additional Context

I filed #3330 a little bit ago, and I believe the fix for this, or at least a commit after v3.0.10 introduced this. That issue will have some more context.

### Debug Output

```console
$ cargo run -- -d 123 -t 123
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:reproducable-example
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Removing generated version
[            clap::build::app] 	App::_propagate_global_args:reproducable-example
[            clap::build::app] 	App::_derive_display_order:reproducable-example
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:device
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:timeout
[clap::build::app::debug_asserts] 	App::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("-d")' ([45, 100])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("-d")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::parse_short_arg: short_arg=RawOsStr("d")
[         clap::parse::parser] 	Parser::parse_short_arg:iter:d
[         clap::parse::parser] 	Parser::parse_short_arg:iter:d: cur_idx:=1
[         clap::parse::parser] 	Parser::parse_short_arg:iter:d: Found valid opt or flag
[         clap::parse::parser] 	Parser::parse_short_arg:iter:d: val=RawOsStr("") (bytes), val=[] (ascii), short_arg=RawOsStr("d")
[         clap::parse::parser] 	Parser::parse_opt; opt=device, val=None
[         clap::parse::parser] 	Parser::parse_opt; opt.settings=ArgFlags(REQUIRED | TAKES_VAL)
[         clap::parse::parser] 	Parser::parse_opt; Checking for val...
[         clap::parse::parser] 	Parser::parse_opt: More arg vals required...
[         clap::parse::parser] 	Parser::remove_overrides: id=device
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_arg: id=device
[            clap::build::app] 	App::groups_for_arg: id=device
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_group: id=usb
[            clap::build::app] 	App::groups_for_arg: id=device
[         clap::parse::parser] 	Parser::get_matches_with: After parse_short_arg Opt(device)
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("123")' ([49, 50, 51])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=device, val=RawOsStr("123")
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."123"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=2
[            clap::build::app] 	App::groups_for_arg: id=device
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=device
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("-t")' ([45, 116])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("-t")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::parse_short_arg: short_arg=RawOsStr("t")
[         clap::parse::parser] 	Parser::parse_short_arg:iter:t
[         clap::parse::parser] 	Parser::parse_short_arg:iter:t: cur_idx:=3
[         clap::parse::parser] 	Parser::parse_short_arg:iter:t: Found valid opt or flag
[         clap::parse::parser] 	Parser::parse_short_arg:iter:t: val=RawOsStr("") (bytes), val=[] (ascii), short_arg=RawOsStr("t")
[         clap::parse::parser] 	Parser::parse_opt; opt=timeout, val=None
[         clap::parse::parser] 	Parser::parse_opt; opt.settings=ArgFlags(REQUIRED | TAKES_VAL)
[         clap::parse::parser] 	Parser::parse_opt; Checking for val...
[         clap::parse::parser] 	Parser::parse_opt: More arg vals required...
[         clap::parse::parser] 	Parser::remove_overrides: id=timeout
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_arg: id=timeout
[            clap::build::app] 	App::groups_for_arg: id=timeout
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_group: id=usb
[            clap::build::app] 	App::groups_for_arg: id=timeout
[         clap::parse::parser] 	Parser::get_matches_with: After parse_short_arg Opt(timeout)
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("123")' ([49, 50, 51])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=timeout, val=RawOsStr("123")
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."123"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=4
[            clap::build::app] 	App::groups_for_arg: id=timeout
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=timeout
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:device:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:device: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:device: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:timeout:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:timeout: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:timeout: doesn't have default missing vals
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::validate_exclusive:iter:device
[      clap::parse::validator] 	Validator::validate_exclusive:iter:usb
[      clap::parse::validator] 	Validator::validate_exclusive:iter:timeout
[      clap::parse::validator] 	Validator::validate_conflicts::iter: id=device
[      clap::parse::validator] 	Conflicts::gather_conflicts
[            clap::build::app] 	App::groups_for_arg: id=device
[      clap::parse::validator] 	Conflicts::gather_direct_conflicts id=device, conflicts=[timeout]
[      clap::parse::validator] 	Conflicts::gather_direct_conflicts id=usb, conflicts=[]
[            clap::build::app] 	App::groups_for_arg: id=timeout
[      clap::parse::validator] 	Conflicts::gather_direct_conflicts id=timeout, conflicts=[device]
[      clap::parse::validator] 	Validator::build_conflict_err: name=device
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_smart_usage
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[device, usb], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={device, timeout}
[         clap::output::usage] 	Usage::get_required_usage_from:iter:device
[         clap::output::usage] 	Usage::get_required_usage_from:iter:timeout
[         clap::output::usage] 	Usage::get_required_usage_from:iter:device
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=["-d <DEVICE>", "-t <TIMEOUT>", "-d <DEVICE>"]
[            clap::build::app] 	App::color: Color setting...
[            clap::build::app] 	Auto
# program output is omitted
```


---

_Label `C-bug` added by @nissaofthesea on 2022-01-25 06:22_

---

_Renamed from "Non-conflicting args " to "Args in same group conflict " by @nissaofthesea on 2022-01-25 06:27_

---

_Comment by @nissaofthesea on 2022-01-25 06:33_

Edit: I think this is a mistake on my part, since the default behavior of `ArgGroup::multiple` is to disallow multiple args from the same group.

---

_Closed by @nissaofthesea on 2022-01-25 06:33_

---

_Comment by @nissaofthesea on 2022-01-25 06:35_

Follow-up question: is there a way to set `ArgGroup::multiple` to `true` using the derive feature? If I put it in the same attribute, it can't find it because it looks for methods from `Arg`.

---

_Comment by @epage on 2022-01-25 13:21_

You can manually define an `ArgGroup`, see https://github.com/clap-rs/clap/blob/master/examples/tutorial_derive/04_03_relations.rs

---
