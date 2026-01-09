---
number: 3556
title: Usage text has duplicate argument when erroring due to conflicting args
type: issue
state: closed
author: chenxiaolong
labels:
  - C-bug
  - E-medium
  - A-validators
assignees: []
created_at: 2022-03-14T23:06:03Z
updated_at: 2022-05-06T01:14:58Z
url: https://github.com/clap-rs/clap/issues/3556
synced_at: 2026-01-07T13:12:19-06:00
---

# Usage text has duplicate argument when erroring due to conflicting args

---

_Issue opened by @chenxiaolong on 2022-03-14 23:06_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.59.0 (9d1b2106e 2022-02-23)

### Clap Version

3.1.6

### Minimal reproducible code

```rust
use clap::{ArgGroup, Parser};

#[derive(Parser)]
#[clap(group(
    ArgGroup::new("group")
        .required(true)
        .args(&["bar", "blah"])
))]
struct Opts {
    #[clap(long)]
    foo: String,
    #[clap(long)]
    bar: Option<String>,
    #[clap(long)]
    blah: bool,
}

fn main() {
    let _ = Opts::parse();
}
```


### Steps to reproduce the bug with the above code

```
cargo run -- --foo value1 --bar value2 --blah
```

### Actual Behaviour

When conflicting arguments are specified, the output is:
```
error: The argument '--bar <BAR>' cannot be used with '--blah'

USAGE:
    clap-duplicate-bug.exe --foo <FOO> --foo <FOO> <--bar <BAR>|--blah>

For more information try --help
```
Note how `--foo <FOO>` shows up twice in the usage text.

### Expected Behaviour

`--foo <FOO>` shouldn't show up twice in the usage text. This seems only happen when there are conflicting arguments. I cannot reproduce it in `--help` or in another error scenario, like missing required args.

### Additional Context

Not sure if it's helpful, but interestingly enough, if omit the `--foo` required arg and only specify the two conflicting options:
```
cargo run -- --bar value2 --blah
```
then the bug does not occur:
```
error: The argument '--bar <BAR>' cannot be used with '--blah'

USAGE:
    clap-duplicate-bug.exe --foo <FOO> <--bar <BAR>|--blah>

For more information try --help
```

### Debug Output

```
[        clap::build::command]  App::_do_parse
[        clap::build::command]  App::_build
[        clap::build::command]  App::_propagate:clap-duplicate-bug
[        clap::build::command]  App::_check_help_and_version: clap-duplicate-bug
[        clap::build::command]  App::_check_help_and_version: Removing generated version
[        clap::build::command]  App::_propagate_global_args:clap-duplicate-bug
[        clap::build::command]  App::_derive_display_order:clap-duplicate-bug
[  clap::build::debug_asserts]  Command::_debug_asserts
[  clap::build::debug_asserts]  Arg::_debug_asserts:help
[  clap::build::debug_asserts]  Arg::_debug_asserts:foo
[  clap::build::debug_asserts]  Arg::_debug_asserts:bar
[  clap::build::debug_asserts]  Arg::_debug_asserts:blah
[  clap::build::debug_asserts]  Command::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsString("--foo")' ([45, 45, 102, 111, 111])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=RawOsStr("--foo")
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::parse_long_arg
[         clap::parse::parser]  Parser::parse_long_arg: cur_idx:=1
[         clap::parse::parser]  Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser]  No
[         clap::parse::parser]  Parser::parse_long_arg: Found valid opt or flag '--foo <FOO>'
[         clap::parse::parser]  Parser::parse_long_arg: Found an opt with value 'None'
[         clap::parse::parser]  Parser::parse_opt; opt=foo, val=None
[         clap::parse::parser]  Parser::parse_opt; opt.settings=ArgFlags(REQUIRED | TAKES_VAL)
[         clap::parse::parser]  Parser::parse_opt; Checking for val...
[         clap::parse::parser]  Parser::parse_opt: More arg vals required...
[         clap::parse::parser]  Parser::remove_overrides: id=foo
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=foo
[        clap::build::command]  App::groups_for_arg: id=foo
[        clap::build::command]  App::groups_for_arg: id=foo
[         clap::parse::parser]  Parser::get_matches_with: After parse_long_arg Opt(foo)
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsString("value1")' ([118, 97, 108, 117, 101, 49])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::add_val_to_arg; arg=foo, val=RawOsStr("value1")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."value1"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=2
[        clap::build::command]  App::groups_for_arg: id=foo
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=foo
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsString("--bar")' ([45, 45, 98, 97, 114])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=RawOsStr("--bar")
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::parse_long_arg
[         clap::parse::parser]  Parser::parse_long_arg: cur_idx:=3
[         clap::parse::parser]  Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser]  No
[         clap::parse::parser]  Parser::parse_long_arg: Found valid opt or flag '--bar <BAR>'
[         clap::parse::parser]  Parser::parse_long_arg: Found an opt with value 'None'
[         clap::parse::parser]  Parser::parse_opt; opt=bar, val=None
[         clap::parse::parser]  Parser::parse_opt; opt.settings=ArgFlags(TAKES_VAL)
[         clap::parse::parser]  Parser::parse_opt; Checking for val...
[         clap::parse::parser]  Parser::parse_opt: More arg vals required...
[         clap::parse::parser]  Parser::remove_overrides: id=bar
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=bar
[        clap::build::command]  App::groups_for_arg: id=bar
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_group: id=group
[        clap::build::command]  App::groups_for_arg: id=bar
[         clap::parse::parser]  Parser::get_matches_with: After parse_long_arg Opt(bar)
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsString("value2")' ([118, 97, 108, 117, 101, 50])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::add_val_to_arg; arg=bar, val=RawOsStr("value2")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."value2"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=4
[        clap::build::command]  App::groups_for_arg: id=bar
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=bar
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsString("--blah")' ([45, 45, 98, 108, 97, 104])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=RawOsStr("--blah")
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::parse_long_arg
[         clap::parse::parser]  Parser::parse_long_arg: cur_idx:=5
[         clap::parse::parser]  Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser]  No
[         clap::parse::parser]  Parser::parse_long_arg: Found valid opt or flag '--blah'
[         clap::parse::parser]  Parser::check_for_help_and_version_str
[         clap::parse::parser]  Parser::check_for_help_and_version_str: Checking if --RawOsStr("blah") is help or version...
[         clap::parse::parser]  Neither
[         clap::parse::parser]  Parser::parse_long_arg: Presence validated
[         clap::parse::parser]  Parser::parse_flag
[         clap::parse::parser]  Parser::remove_overrides: id=blah
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=blah
[        clap::build::command]  App::groups_for_arg: id=blah
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_group: id=group
[         clap::parse::parser]  Parser::get_matches_with: After parse_long_arg ValuesDone
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_defaults
[         clap::parse::parser]  Parser::add_defaults:iter:foo:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:foo: doesn't have default vals
[         clap::parse::parser]  Parser::add_value:iter:foo: doesn't have default missing vals
[         clap::parse::parser]  Parser::add_defaults:iter:bar:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:bar: doesn't have default vals
[         clap::parse::parser]  Parser::add_value:iter:bar: doesn't have default missing vals
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::validate_exclusive:iter:foo
[      clap::parse::validator]  Validator::validate_exclusive:iter:bar
[      clap::parse::validator]  Validator::validate_exclusive:iter:group
[      clap::parse::validator]  Validator::validate_exclusive:iter:blah
[      clap::parse::validator]  Validator::validate_conflicts::iter: id=foo
[      clap::parse::validator]  Conflicts::gather_conflicts
[        clap::build::command]  App::groups_for_arg: id=foo
[      clap::parse::validator]  Conflicts::gather_direct_conflicts id=foo, conflicts=[]
[        clap::build::command]  App::groups_for_arg: id=bar
[      clap::parse::validator]  Conflicts::gather_direct_conflicts id=bar, conflicts=[blah]
[      clap::parse::validator]  Conflicts::gather_direct_conflicts id=group, conflicts=[]
[        clap::build::command]  App::groups_for_arg: id=blah
[      clap::parse::validator]  Conflicts::gather_direct_conflicts id=blah, conflicts=[bar]
[      clap::parse::validator]  Validator::validate_conflicts::iter: id=bar
[      clap::parse::validator]  Conflicts::gather_conflicts
[      clap::parse::validator]  Validator::build_conflict_err: name=bar
[         clap::output::usage]  Usage::create_usage_with_title
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_smart_usage
[         clap::output::usage]  Usage::get_required_usage_from: incls=[foo, bar, group], matcher=false, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={foo, group}
[        clap::build::command]  App::unroll_args_in_group: group=group
[        clap::build::command]  App::unroll_args_in_group:iter: entity=bar
[        clap::build::command]  App::unroll_args_in_group:iter: this is an arg
[        clap::build::command]  App::unroll_args_in_group:iter: entity=blah
[        clap::build::command]  App::unroll_args_in_group:iter: this is an arg
[         clap::output::usage]  Usage::get_required_usage_from:iter:foo
[         clap::output::usage]  Usage::get_required_usage_from:iter:foo
[        clap::build::command]  App::unroll_args_in_group: group=group
[        clap::build::command]  App::unroll_args_in_group:iter: entity=bar
[        clap::build::command]  App::unroll_args_in_group:iter: this is an arg
[        clap::build::command]  App::unroll_args_in_group:iter: entity=blah
[        clap::build::command]  App::unroll_args_in_group:iter: this is an arg
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=["--foo <FOO>", "--foo <FOO>", "<--bar <BAR>|--blah>"]
[        clap::build::command]  App::color: Color setting...
[        clap::build::command]  Auto
error: The argument '--bar <BAR>' cannot be used with '--blah'

USAGE:
    clap-duplicate-bug.exe --foo <FOO> --foo <FOO> <--bar <BAR>|--blah>

For more information try --help
```

---

_Label `C-bug` added by @chenxiaolong on 2022-03-14 23:06_

---

_Label `E-medium` added by @epage on 2022-03-15 01:12_

---

_Label `A-validators` added by @epage on 2022-03-15 01:12_

---

_Comment by @epage on 2022-03-15 01:12_

Thanks for reporting this!

---

_Referenced in [clap-rs/clap#3665](../../clap-rs/clap/issues/3665.md) on 2022-04-29 15:50_

---

_Added to milestone `3.x` by @epage on 2022-04-29 15:50_

---

_Referenced in [clap-rs/clap#3688](../../clap-rs/clap/pulls/3688.md) on 2022-05-04 16:03_

---

_Referenced in [clap-rs/clap#3689](../../clap-rs/clap/pulls/3689.md) on 2022-05-04 16:07_

---

_Closed by @epage on 2022-05-06 00:52_

---

_Comment by @chenxiaolong on 2022-05-06 01:14_

Thanks for the fix!

---
