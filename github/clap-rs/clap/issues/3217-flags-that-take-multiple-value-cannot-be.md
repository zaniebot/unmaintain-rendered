---
number: 3217
title: Flags that take multiple value cannot be partially overridden
type: issue
state: closed
author: ducaale
labels:
  - C-bug
  - A-parsing
  - E-medium
assignees: []
created_at: 2021-12-25T13:43:18Z
updated_at: 2021-12-27T21:59:39Z
url: https://github.com/clap-rs/clap/issues/3217
synced_at: 2026-01-07T13:12:19-06:00
---

# Flags that take multiple value cannot be partially overridden

---

_Issue opened by @ducaale on 2021-12-25 13:43_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.56.0 (09c42c458 2021-10-18)

### Clap Version

3.0.0-rc.8

### Minimal reproducible code

```rust
use clap::Parser;

/// Simple program to greet a person
#[derive(Parser, Debug)]
struct Args {
    /// Number of times to greet
    #[clap(short, long, number_of_values = 1)]
    name: Vec<String>,

    #[clap(long, overrides_with = "name")]
    no_name: bool,
}

fn main() {
    let _ = dbg!(Args::parse());
}
```


### Steps to reproduce the bug with the above code

```
cargo run -- --name=ahmed --no-name --name=ali
```

### Actual Behaviour
`--no-name` is not unsetting the previous values of `--name`
```
[src/main.rs:15] Args::parse() = Args {
    name: [
        "ahmed",
        "ali",
    ],
    no_name: false,
}
```

### Expected Behaviour

From Clap 2.x/StructOpt
```
[src/main.rs:16] Args::from_args() = Args {
    name: [
        "ali",
    ],
    no_name: false,
}
```

### Additional Context

No response

### Debug Output

No response

---

_Label `C-bug` added by @ducaale on 2021-12-25 13:43_

---

_Label `A-parsing` added by @epage on 2021-12-25 16:45_

---

_Label `S-triage` added by @epage on 2021-12-25 16:45_

---

_Added to milestone `3.0` by @epage on 2021-12-25 16:45_

---

_Comment by @epage on 2021-12-25 16:45_

Thanks for reporting this!

---

_Comment by @epage on 2021-12-26 01:41_

Alright, got a local reproduction of the before/after.

For easily switching between structopt and clap3, I used this code
```rust
//use clap::StructOpt;
use structopt::StructOpt;

/// Simple program to greet a person
#[derive(StructOpt, Debug)]
struct Args {
    /// Number of times to greet
    #[structopt(short, long, number_of_values = 1)]
    name: Vec<String>,

    #[structopt(long, overrides_with = "name")]
    no_name: bool,
}

fn main() {
    let _ = dbg!(Args::from_args());
}
```

---

_Label `S-triage` removed by @epage on 2021-12-26 01:42_

---

_Label `E-medium` added by @epage on 2021-12-26 01:42_

---

_Comment by @epage on 2021-12-26 01:44_

clap2 debug output:
```
DEBUG:clap:Parser::propagate_settings: self=test-clap, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--name=ahmed"' ([45, 45, 110, 97, 109, 101, 61, 97, 104, 109, 101, 100])
DEBUG:clap:Parser::is_new_arg:"--name=ahmed":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="--name=ahmed"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...Yes '"ahmed"'
DEBUG:clap:OptBuilder::fmt:name
DEBUG:clap:Parser::parse_long_arg: Found valid opt '--name <name>...'
DEBUG:clap:Parser::parse_opt; opt=name, val=Some("ahmed")
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(MULTIPLE | EMPTY_VALS | TAKES_VAL | DELIM_NOT_SET)
DEBUG:clap:Parser::parse_opt; Checking for val...Found - "ahmed", len: 5
DEBUG:clap:Parser::parse_opt: "ahmed" contains '='...false
DEBUG:clap:Parser::add_val_to_arg; arg=name, val="ahmed"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."ahmed"
DEBUG:clap:Parser::groups_for_arg: name=name
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=name
DEBUG:clap:ArgMatcher::needs_more_vals: num_vals...1
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=name
DEBUG:clap:Parser::groups_for_arg: name=name
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::parse_opt: More arg vals not required...
DEBUG:clap:Parser:get_matches_with: After parse_long_arg ValuesDone
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--no-name"' ([45, 45, 110, 111, 45, 110, 97, 109, 101])
DEBUG:clap:Parser::is_new_arg:"--no-name":ValuesDone
DEBUG:clap:Parser::possible_subcommand: arg="--no-name"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:Some("name");
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:Parser::parse_long_arg: Found valid flag '--no-name'
DEBUG:clap:Parser::check_for_help_and_version_str;
DEBUG:clap:Parser::check_for_help_and_version_str: Checking if --no-name is help or version...Neither
DEBUG:clap:Parser::parse_flag;
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=no-name
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=no-name
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser:get_matches_with: After parse_long_arg Flag
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--name=ali"' ([45, 45, 110, 97, 109, 101, 61, 97, 108, 105])
DEBUG:clap:Parser::is_new_arg:"--name=ali":Flag
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="--name=ali"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:Some("no-name");
DEBUG:clap:ArgMatcher::process_arg_overrides:iter:name;
DEBUG:clap:ArgMatcher::process_arg_overrides:iter:name: removing from matches;
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...Yes '"ali"'
DEBUG:clap:OptBuilder::fmt:name
DEBUG:clap:Parser::parse_long_arg: Found valid opt '--name <name>...'
DEBUG:clap:Parser::parse_opt; opt=name, val=Some("ali")
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(MULTIPLE | EMPTY_VALS | TAKES_VAL | DELIM_NOT_SET)
DEBUG:clap:Parser::parse_opt; Checking for val...Found - "ali", len: 3
DEBUG:clap:Parser::parse_opt: "ali" contains '='...false
DEBUG:clap:Parser::add_val_to_arg; arg=name, val="ali"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."ali"
DEBUG:clap:Parser::groups_for_arg: name=name
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=name
DEBUG:clap:ArgMatcher::needs_more_vals: num_vals...1
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=name
DEBUG:clap:Parser::groups_for_arg: name=name
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::parse_opt: More arg vals not required...
DEBUG:clap:Parser:get_matches_with: After parse_long_arg ValuesDone
DEBUG:clap:ArgMatcher::process_arg_overrides:Some("name");
DEBUG:clap:Parser::remove_overrides:[("name", "no-name")];
DEBUG:clap:Parser::remove_overrides:iter:(name,no-name);
DEBUG:clap:Parser::remove_overrides:iter:(name,no-name): removing no-name;
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Parser::add_defaults:iter:name: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:name: doesn't have default vals
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_blacklist:iter:name;
DEBUG:clap:Parser::groups_for_arg: name=name
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:name: vals=[
    "ali",
]
DEBUG:clap:Validator::validate_arg_num_vals:name
DEBUG:clap:Validator::validate_arg_num_vals: num_vals set...1
DEBUG:clap:Validator::validate_arg_values: arg="name"
DEBUG:clap:Validator::validate_arg_values: checking validator...good
DEBUG:clap:Validator::validate_arg_requires:name;
DEBUG:clap:Validator::validate_arg_num_occurs: a=name;
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=no-name;
DEBUG:clap:Parser::groups_for_arg: name=no-name
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:usage::needs_flags_tag:iter: [FLAGS] required
DEBUG:clap:usage::create_help_usage: usage=test-clap [FLAGS] [OPTIONS]
DEBUG:clap:ArgMatcher::get_global_values: global_arg_vec=[]
```

---

_Comment by @epage on 2021-12-26 01:45_

clap3 debug output:
```
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:test-clap
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Removing generated version
[            clap::build::app] 	App::_propagate_global_args:test-clap
[            clap::build::app] 	App::_derive_display_order:test-clap
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:name
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:no-name
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--name=ahmed")' ([45, 45, 110, 97, 109, 101, 61, 97, 104, 109, 101, 100])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("--name=ahmed")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::parse_long_arg
[         clap::parse::parser] 	Parser::parse_long_arg: cur_idx:=1
[         clap::parse::parser] 	Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser] 	Yes 'RawOsStr("=ahmed")'
[         clap::parse::parser] 	Parser::parse_long_arg: Found valid opt or flag '--name <NAME>'
[         clap::parse::parser] 	Parser::parse_long_arg: Found an opt with value 'Some(RawOsStr("=ahmed"))'
[         clap::parse::parser] 	Parser::parse_opt; opt=name, val=Some(RawOsStr("=ahmed"))
[         clap::parse::parser] 	Parser::parse_opt; opt.settings=ArgFlags(MULTIPLE_OCC | TAKES_VAL | MULTIPLE_VALS | MULTIPLE)
[         clap::parse::parser] 	Parser::parse_opt; Checking for val...
[         clap::parse::parser] 	Found - RawOsStr("ahmed"), len: 5
[         clap::parse::parser] 	Parser::parse_opt: RawOsStr("=ahmed") contains '='...true
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=name, val=RawOsStr("ahmed")
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."ahmed"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=2
[            clap::build::app] 	App::groups_for_arg: id=name
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=name
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: num_vals...1
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_arg: id=name
[            clap::build::app] 	App::groups_for_arg: id=name
[         clap::parse::parser] 	Parser::get_matches_with: After parse_long_arg ValuesDone
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--no-name")' ([45, 45, 110, 111, 45, 110, 97, 109, 101])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("--no-name")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::parse_long_arg
[         clap::parse::parser] 	Parser::parse_long_arg: cur_idx:=3
[         clap::parse::parser] 	Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser] 	No
[         clap::parse::parser] 	Parser::parse_long_arg: Found valid opt or flag '--no-name'
[         clap::parse::parser] 	Parser::check_for_help_and_version_str
[         clap::parse::parser] 	Parser::check_for_help_and_version_str: Checking if --RawOsStr("no-name") is help or version...
[         clap::parse::parser] 	Neither
[         clap::parse::parser] 	Parser::parse_long_arg: Presence validated
[         clap::parse::parser] 	Parser::parse_flag
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_arg: id=no-name
[            clap::build::app] 	App::groups_for_arg: id=no-name
[         clap::parse::parser] 	Parser::get_matches_with: After parse_long_arg ValuesDone
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--name=ali")' ([45, 45, 110, 97, 109, 101, 61, 97, 108, 105])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("--name=ali")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::parse_long_arg
[         clap::parse::parser] 	Parser::parse_long_arg: cur_idx:=4
[         clap::parse::parser] 	Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser] 	Yes 'RawOsStr("=ali")'
[         clap::parse::parser] 	Parser::parse_long_arg: Found valid opt or flag '--name <NAME>'
[         clap::parse::parser] 	Parser::parse_long_arg: Found an opt with value 'Some(RawOsStr("=ali"))'
[         clap::parse::parser] 	Parser::parse_opt; opt=name, val=Some(RawOsStr("=ali"))
[         clap::parse::parser] 	Parser::parse_opt; opt.settings=ArgFlags(MULTIPLE_OCC | TAKES_VAL | MULTIPLE_VALS | MULTIPLE)
[         clap::parse::parser] 	Parser::parse_opt; Checking for val...
[         clap::parse::parser] 	Found - RawOsStr("ali"), len: 3
[         clap::parse::parser] 	Parser::parse_opt: RawOsStr("=ali") contains '='...true
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=name, val=RawOsStr("ali")
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."ali"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=5
[            clap::build::app] 	App::groups_for_arg: id=name
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=name
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: num_vals...1
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_arg: id=name
[            clap::build::app] 	App::groups_for_arg: id=name
[         clap::parse::parser] 	Parser::get_matches_with: After parse_long_arg ValuesDone
[         clap::parse::parser] 	Parser::remove_overrides
[         clap::parse::parser] 	Parser::remove_overrides:iter:name
[         clap::parse::parser] 	Parser::remove_overrides:iter:no-name
[         clap::parse::parser] 	Parser::remove_overrides:iter:no-name => Arg { id: no-name, provider: User, name: "no-name", help: None, long_help: None, blacklist: [], settings: ArgFlags(NO_OP), overrides: [name], groups: [], requires: [], r_ifs: [], r_unless: [], short: None, long: Some("no-name"), aliases: [], short_aliases: [], disp_ord: None, possible_vals: [], val_names: [], num_vals: None, max_vals: None, min_vals: None, validator: "None", validator_os: "None", val_delim: None, default_vals: [], default_vals_ifs: [], terminator: None, index: None, help_heading: Some(None), value_hint: Unknown, default_missing_vals: [] }
[         clap::parse::parser] 	Parser::remove_overrides:iter:no-name: removing
[         clap::parse::parser] 	Parser::remove_overrides:iter:no-name: removing
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:name:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:name: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:name: doesn't have default missing vals
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::validate_exclusive:iter:name
[      clap::parse::validator] 	Validator::validate_conflicts::iter: id=name
[      clap::parse::validator] 	Conflicts::gather_conflicts
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::gather_requirements:iter:name
[      clap::parse::validator] 	Validator::validate_required_unless
[      clap::parse::validator] 	Validator::validate_matched_args
[      clap::parse::validator] 	Validator::validate_matched_args:iter:name: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "ahmed",
                        ],
                        [
                            "ali",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_num_vals: num_vals set...1
[      clap::parse::validator] 	Validator::validate_arg_values: arg="name"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "name"=2
[    clap::parse::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[help]
```

---

_Comment by @epage on 2021-12-26 02:01_

7673dfc08 is likely when this was broken, when we switched overrides from being eager to being lazy.

That commit and its message is the most we'll get.  #1154 doesn't add much context.  The PR description is generic and there was no review.

---

_Referenced in [clap-rs/clap#3223](../../clap-rs/clap/pulls/3223.md) on 2021-12-27 18:57_

---

_Referenced in [clap-rs/clap#3225](../../clap-rs/clap/pulls/3225.md) on 2021-12-27 21:33_

---

_Closed by @epage on 2021-12-27 21:55_

---

_Comment by @epage on 2021-12-27 21:59_

Just released 3.0.0-rc.9 with this fix

---
