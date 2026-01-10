---
number: 3665
title: Duplicate output in USAGE message if called with conflict argument
type: issue
state: closed
author: leolovenet
labels:
  - C-bug
  - A-help
  - E-easy
assignees: []
created_at: 2022-04-29T15:40:36Z
updated_at: 2022-05-04T13:39:00Z
url: https://github.com/clap-rs/clap/issues/3665
synced_at: 2026-01-10T01:27:44Z
---

# Duplicate output in USAGE message if called with conflict argument

---

_Issue opened by @leolovenet on 2022-04-29 15:40_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.60.0 (7737e0b5c 2022-04-04)

### Clap Version

3.1.12

### Minimal reproducible code

```rust
use clap::{Command, Arg};

fn main() {
    let _matches = Command::new("Clap-created-USAGE-string-bug")
        .arg(
            Arg::new("a").required(true)
        )
        .arg(
            Arg::new("b")
                .short('b')
                .takes_value(true)
                .conflicts_with("c")
                // .group("debug")
        )
        .arg(
            Arg::new("c")
                .short('c')
                .takes_value(true)
                .conflicts_with("b")
                // .group("debug")
        )
        .get_matches();
}
```


### Steps to reproduce the bug with the above code

 `cargo run -- aaa -b bbb -c ccc`

### Actual Behaviour

```
error: The argument '-b <b>' cannot be used with '-c <c>'

USAGE:
    clap-usage-bug -b <b> <a> <a>

For more information try --help
```

### Expected Behaviour

```
error: The argument '-b <b>' cannot be used with '-c <c>'

USAGE:
    clap-usage-bug -b <b> <a>

For more information try --help
```

### Additional Context

_No response_

### Debug Output

```
[        clap::build::command]  App::_do_parse
[        clap::build::command]  App::_build
[        clap::build::command]  App::_propagate:Clap-created-USAGE-string-bug
[        clap::build::command]  App::_check_help_and_version: Clap-created-USAGE-string-bug
[        clap::build::command]  App::_check_help_and_version: Removing generated version
[        clap::build::command]  App::_propagate_global_args:Clap-created-USAGE-string-bug
[        clap::build::command]  App::_derive_display_order:Clap-created-USAGE-string-bug
[  clap::build::debug_asserts]  Command::_debug_asserts
[  clap::build::debug_asserts]  Arg::_debug_asserts:help
[  clap::build::debug_asserts]  Arg::_debug_asserts:a
[  clap::build::debug_asserts]  Arg::_debug_asserts:b
[  clap::build::debug_asserts]  Arg::_debug_asserts:c
[  clap::build::debug_asserts]  Command::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("aaa")' ([97, 97, 97])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=Ok("aaa")
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::remove_overrides: id=a
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=a
[        clap::build::command]  App::groups_for_arg: id=a
[         clap::parse::parser]  Parser::add_val_to_arg; arg=a, val=RawOsStr("aaa")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."aaa"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=1
[        clap::build::command]  App::groups_for_arg: id=a
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=a
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("-b")' ([45, 98])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...2
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=Ok("-b")
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("b"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['b']) }, invalid_suffix: None }
[         clap::parse::parser]  Parser::parse_short_arg:iter:b
[         clap::parse::parser]  Parser::parse_short_arg:iter:b: cur_idx:=2
[         clap::parse::parser]  Parser::parse_short_arg:iter:b: Found valid opt or flag
[         clap::parse::parser]  Parser::parse_short_arg:iter:b: val=RawOsStr("") (bytes), val=[] (ascii), short_arg=ShortFlags { inner: RawOsStr("b"), utf8_prefix: CharIndices { front_offset: 1, iter: Chars([]) }, invalid_suffix: None }
[         clap::parse::parser]  Parser::parse_opt; opt=b, val=None, has_eq=false
[         clap::parse::parser]  Parser::parse_opt; opt.settings=ArgFlags(TAKES_VAL)
[         clap::parse::parser]  Parser::parse_opt; Checking for val...
[         clap::parse::parser]  Parser::parse_opt: More arg vals required...
[         clap::parse::parser]  Parser::remove_overrides: id=b
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=b
[        clap::build::command]  App::groups_for_arg: id=b
[        clap::build::command]  App::groups_for_arg: id=b
[         clap::parse::parser]  Parser::get_matches_with: After parse_short_arg Opt(b)
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("bbb")' ([98, 98, 98])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...2
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::add_val_to_arg; arg=b, val=RawOsStr("bbb")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."bbb"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=3
[        clap::build::command]  App::groups_for_arg: id=b
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=b
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("-c")' ([45, 99])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...2
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=Ok("-c")
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("c"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['c']) }, invalid_suffix: None }
[         clap::parse::parser]  Parser::parse_short_arg:iter:c
[         clap::parse::parser]  Parser::parse_short_arg:iter:c: cur_idx:=4
[         clap::parse::parser]  Parser::parse_short_arg:iter:c: Found valid opt or flag
[         clap::parse::parser]  Parser::parse_short_arg:iter:c: val=RawOsStr("") (bytes), val=[] (ascii), short_arg=ShortFlags { inner: RawOsStr("c"), utf8_prefix: CharIndices { front_offset: 1, iter: Chars([]) }, invalid_suffix: None }
[         clap::parse::parser]  Parser::parse_opt; opt=c, val=None, has_eq=false
[         clap::parse::parser]  Parser::parse_opt; opt.settings=ArgFlags(TAKES_VAL)
[         clap::parse::parser]  Parser::parse_opt; Checking for val...
[         clap::parse::parser]  Parser::parse_opt: More arg vals required...
[         clap::parse::parser]  Parser::remove_overrides: id=c
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=c
[        clap::build::command]  App::groups_for_arg: id=c
[        clap::build::command]  App::groups_for_arg: id=c
[         clap::parse::parser]  Parser::get_matches_with: After parse_short_arg Opt(c)
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("ccc")' ([99, 99, 99])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...2
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::add_val_to_arg; arg=c, val=RawOsStr("ccc")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."ccc"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=5
[        clap::build::command]  App::groups_for_arg: id=c
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=c
[         clap::parse::parser]  Parser::add_defaults
[         clap::parse::parser]  Parser::add_defaults:iter:b:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:b: doesn't have default vals
[         clap::parse::parser]  Parser::add_value:iter:b: doesn't have default missing vals
[         clap::parse::parser]  Parser::add_defaults:iter:c:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:c: doesn't have default vals
[         clap::parse::parser]  Parser::add_value:iter:c: doesn't have default missing vals
[         clap::parse::parser]  Parser::add_defaults:iter:a:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:a: doesn't have default vals
[         clap::parse::parser]  Parser::add_value:iter:a: doesn't have default missing vals
[      clap::parse::validator]  Validator::validate
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::validate_exclusive:iter:a
[      clap::parse::validator]  Validator::validate_exclusive:iter:b
[      clap::parse::validator]  Validator::validate_exclusive:iter:c
[      clap::parse::validator]  Validator::validate_conflicts::iter: id=a
[      clap::parse::validator]  Conflicts::gather_conflicts
[        clap::build::command]  App::groups_for_arg: id=a
[      clap::parse::validator]  Conflicts::gather_direct_conflicts id=a, conflicts=[]
[        clap::build::command]  App::groups_for_arg: id=b
[      clap::parse::validator]  Conflicts::gather_direct_conflicts id=b, conflicts=[c]
[        clap::build::command]  App::groups_for_arg: id=c
[      clap::parse::validator]  Conflicts::gather_direct_conflicts id=c, conflicts=[b]
[      clap::parse::validator]  Validator::validate_conflicts::iter: id=b
[      clap::parse::validator]  Conflicts::gather_conflicts
[      clap::parse::validator]  Validator::build_conflict_err: name=b
[         clap::output::usage]  Usage::create_usage_with_title
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_smart_usage
[         clap::output::usage]  Usage::get_required_usage_from: incls=[a, b], matcher=false, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={a}
[         clap::output::usage]  Usage::get_required_usage_from:iter:b
[         clap::output::usage]  Usage::get_required_usage_from:iter:a
[         clap::output::usage]  Usage::get_required_usage_from:iter:a
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=["-b <b>", "<a>", "<a>"]
[        clap::build::command]  App::color: Color setting...
[        clap::build::command]  Auto
error: The argument '-b <b>' cannot be used with '-c <c>'

USAGE:
    clap-usage-bug -b <b> <a> <a>

For more information try --help
```

---

_Label `C-bug` added by @leolovenet on 2022-04-29 15:40_

---

_Label `A-help` added by @epage on 2022-04-29 15:49_

---

_Label `E-easy` added by @epage on 2022-04-29 15:49_

---

_Comment by @epage on 2022-04-29 15:50_

I suspect the root cause is the same as #3556 though the exact configuration is slightly different.

---

_Added to milestone `3.x` by @epage on 2022-04-29 15:50_

---

_Comment by @leolovenet on 2022-05-04 13:39_

Sorry, I rechecked and it is indeed the same problem, so I will close this issese

---

_Closed by @leolovenet on 2022-05-04 13:39_

---

_Referenced in [clap-rs/clap#3688](../../clap-rs/clap/pulls/3688.md) on 2022-05-04 16:03_

---
