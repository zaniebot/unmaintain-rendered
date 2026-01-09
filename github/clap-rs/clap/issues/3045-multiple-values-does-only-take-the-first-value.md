---
number: 3045
title: "`multiple_values` does only take the first value"
type: issue
state: closed
author: b1rger
labels:
  - C-bug
assignees: []
created_at: 2021-11-22T19:36:45Z
updated_at: 2021-11-22T19:43:17Z
url: https://github.com/clap-rs/clap/issues/3045
synced_at: 2026-01-07T13:12:19-06:00
---

# `multiple_values` does only take the first value

---

_Issue opened by @b1rger on 2021-11-22 19:36_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.56.0

### Clap Version

3.0.0-beta.5

### Minimal reproducible code

```rust
use clap::{Parser};

#[derive(Parser)]
struct Opts {
    #[clap(multiple_values=true, takes_value=true)]
    date: Option<String>,
}

fn main() {
    let opts: Opts = Opts::parse();
    println!("{:?}", opts.date);
}

```


### Steps to reproduce the bug with the above code

cargo run -- 2020 03 05

### Actual Behaviour

Some("2020")

### Expected Behaviour

Some("2020 03 05") or maybe a vector?

### Additional Context

_No response_

### Debug Output

[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:claptest
[            clap::build::app]  App::_check_help_and_version
[            clap::build::app]  App::_check_help_and_version: Removing generated version
[            clap::build::app]  App::_propagate_global_args:claptest
[            clap::build::app]  App::_derive_display_order:claptest
[clap::build::app::debug_asserts]       App::_debug_asserts
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:help
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:date
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("2020")' ([50, 48, 50, 48])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=RawOsStr("2020")
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::add_val_to_arg; arg=date, val=RawOsStr("2020")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."2020"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=1
[            clap::build::app]  App::groups_for_arg: id=date
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=date
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of: arg=date
[            clap::build::app]  App::groups_for_arg: id=date
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("03")' ([48, 51])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::add_val_to_arg; arg=date, val=RawOsStr("03")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."03"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=2
[            clap::build::app]  App::groups_for_arg: id=date
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=date
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of: arg=date
[            clap::build::app]  App::groups_for_arg: id=date
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("05")' ([48, 53])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::add_val_to_arg; arg=date, val=RawOsStr("05")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."05"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=3
[            clap::build::app]  App::groups_for_arg: id=date
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=date
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of: arg=date
[            clap::build::app]  App::groups_for_arg: id=date
[         clap::parse::parser]  Parser::remove_overrides
[         clap::parse::parser]  Parser::remove_overrides:iter:date
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_env: Checking arg `--help`
[         clap::parse::parser]  Parser::add_env: Skipping existing arg `<DATE>...`
[         clap::parse::parser]  Parser::add_defaults
[         clap::parse::parser]  Parser::add_defaults:iter:date:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:date: doesn't have default vals
[         clap::parse::parser]  Parser::add_value:iter:date: doesn't have default missing vals
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::validate_exclusive:iter:date
[      clap::parse::validator]  Validator::gather_conflicts
[      clap::parse::validator]  Validator::gather_conflicts:iter: id=date
[            clap::build::app]  App::groups_for_arg: id=date
[      clap::parse::validator]  Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator]  Validator::gather_requirements
[      clap::parse::validator]  Validator::gather_requirements:iter:date
[      clap::parse::validator]  Validator::validate_required_unless
[      clap::parse::validator]  Validator::validate_matched_args
[      clap::parse::validator]  Validator::validate_matched_args:iter:date: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "2020",
                            "03",
                            "05",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator]  Validator::validate_arg_num_vals
[      clap::parse::validator]  Validator::validate_arg_values: arg="date"
[      clap::parse::validator]  Validator::validate_arg_values: checking validator...
[      clap::parse::validator]  good
[      clap::parse::validator]  Validator::validate_arg_values: checking validator...
[      clap::parse::validator]  good
[      clap::parse::validator]  Validator::validate_arg_values: checking validator...
[      clap::parse::validator]  good
[      clap::parse::validator]  Validator::validate_arg_requires:"date"
[      clap::parse::validator]  Validator::validate_arg_num_occurs: "date"=3
[    clap::parse::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[help]
Some("2020")

---

_Label `T: bug` added by @b1rger on 2021-11-22 19:36_

---

_Comment by @mkayaalp on 2021-11-22 19:41_

The type needs to be a `Vec<T>`:
```diff
-     date: Option<String>,
+     date: Vec<String>,
```

---

_Comment by @b1rger on 2021-11-22 19:43_

> The type needs to be a Vec<T>

Ah, oke, I didn't think of that! Thanks!


---

_Closed by @b1rger on 2021-11-22 19:43_

---
