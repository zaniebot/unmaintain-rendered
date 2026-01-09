---
number: 3412
title: Derive ArgGroup errors when arguments take values
type: issue
state: closed
author: malthesr
labels:
  - C-bug
assignees: []
created_at: 2022-02-07T14:46:27Z
updated_at: 2022-02-07T14:51:23Z
url: https://github.com/clap-rs/clap/issues/3412
synced_at: 2026-01-07T13:12:19-06:00
---

# Derive ArgGroup errors when arguments take values

---

_Issue opened by @malthesr on 2022-02-07 14:46_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.58.1 (db9d1b20b 2022-01-20)

### Clap Version

3.0.14

### Minimal reproducible code

```rust
use std::path::PathBuf;

use clap::{ArgGroup, Parser};

#[derive(Debug, Parser)]
#[clap(group(ArgGroup::new("mygroup").required(true)))]
struct Cli {
    #[clap(long, group = "mygroup")]
    mypath: PathBuf,

    #[clap(long, group = "mygroup")]
    myusize: usize,
}

fn main() {
    let cli = Cli::parse();

    println!("{cli:#?}");
}
```


### Steps to reproduce the bug with the above code

``` shell
cargo run -- --myusize 1
```

### Actual Behaviour

When I run `cargo run -- --myusize 1`, `clap` reports the following error:

```
error: The following required arguments were not provided:

USAGE:
    playground <--mypath <MYPATH>|--myusize <MYUSIZE>>

For more information try --help
```

The same happens if I run `cargo run -- --mypath some/path`. 


### Expected Behaviour

I thought this would create a group such that `--myusize` could be set, or `--mypath` could be set, but not neither and not both. Instead, it fails if neither is set, if exactly one of them is set, or if both are set.

The error reported by clap also seems to miss some context: it says the "following" required arguments were not provided, but lists none.



### Additional Context

This seems to work as expected with `bool`, as also demonstrated by the relevant example [`clap/04_03_relations.rs`](https://github.com/clap-rs/clap/blob/master/examples/tutorial_derive/04_03_relations.rs). E.g., the following version seems to work as I'd expect (except of course that I can't provide any values):

``` rust
use clap::{ArgGroup, Parser};

#[derive(Debug, Parser)]
#[clap(group(ArgGroup::new("mygroup").required(true)))]
struct Cli {
    #[clap(long, group = "mygroup")]
    mypath: bool,

    #[clap(long, group = "mygroup")]
    myusize: bool,
}

fn main() {
    let cli = Cli::parse();

    println!("{cli:#?}");
}
```

### Debug Output

```
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:playground
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Removing generated version
[            clap::build::app] 	App::_propagate_global_args:playground
[            clap::build::app] 	App::_derive_display_order:playground
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:mypath
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:myusize
[clap::build::app::debug_asserts] 	App::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--myusize")' ([45, 45, 109, 121, 117, 115, 105, 122, 101])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("--myusize")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::parse_long_arg
[         clap::parse::parser] 	Parser::parse_long_arg: cur_idx:=1
[         clap::parse::parser] 	Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser] 	No
[         clap::parse::parser] 	Parser::parse_long_arg: Found valid opt or flag '--myusize <MYUSIZE>'
[         clap::parse::parser] 	Parser::parse_long_arg: Found an opt with value 'None'
[         clap::parse::parser] 	Parser::parse_opt; opt=myusize, val=None
[         clap::parse::parser] 	Parser::parse_opt; opt.settings=ArgFlags(REQUIRED | TAKES_VAL)
[         clap::parse::parser] 	Parser::parse_opt; Checking for val...
[         clap::parse::parser] 	Parser::parse_opt: More arg vals required...
[         clap::parse::parser] 	Parser::remove_overrides: id=myusize
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_arg: id=myusize
[            clap::build::app] 	App::groups_for_arg: id=myusize
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_group: id=mygroup
[            clap::build::app] 	App::groups_for_arg: id=myusize
[         clap::parse::parser] 	Parser::get_matches_with: After parse_long_arg Opt(myusize)
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("1")' ([49])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=myusize, val=RawOsStr("1")
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."1"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=2
[            clap::build::app] 	App::groups_for_arg: id=myusize
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=myusize
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:mypath:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:mypath: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:mypath: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:myusize:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:myusize: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:myusize: doesn't have default missing vals
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::validate_exclusive:iter:myusize
[      clap::parse::validator] 	Validator::validate_exclusive:iter:mygroup
[      clap::parse::validator] 	Validator::validate_conflicts::iter: id=myusize
[      clap::parse::validator] 	Conflicts::gather_conflicts
[            clap::build::app] 	App::groups_for_arg: id=myusize
[      clap::parse::validator] 	Conflicts::gather_direct_conflicts id=myusize, conflicts=[mypath]
[      clap::parse::validator] 	Conflicts::gather_direct_conflicts id=mygroup, conflicts=[]
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([Child { id: mypath, children: [] }, Child { id: myusize, children: [] }, Child { id: mygroup, children: [] }])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::gather_requirements:iter:myusize
[      clap::parse::validator] 	Validator::gather_requirements:iter:mygroup
[      clap::parse::validator] 	Validator::gather_requirements:iter:mygroup:group
[      clap::parse::validator] 	Validator::validate_required:iter:aog=mypath
[      clap::parse::validator] 	Validator::validate_required:iter: This is an arg
[      clap::parse::validator] 	Validator::is_missing_required_ok: mypath
[      clap::parse::validator] 	Validator::validate_arg_conflicts: a="mypath"
[      clap::parse::validator] 	Validator::missing_required_error; incl=[]
[      clap::parse::validator] 	Validator::missing_required_error: reqs=ChildGraph([Child { id: mypath, children: [] }, Child { id: myusize, children: [] }, Child { id: mygroup, children: [] }])
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=true, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={mypath, myusize, mygroup}
[            clap::build::app] 	App::unroll_args_in_group: group=mygroup
[            clap::build::app] 	App::unroll_args_in_group:iter: entity=mypath
[            clap::build::app] 	App::unroll_args_in_group:iter: this is an arg
[            clap::build::app] 	App::unroll_args_in_group:iter: entity=myusize
[            clap::build::app] 	App::unroll_args_in_group:iter: this is an arg
[            clap::build::app] 	App::unroll_args_in_group: group=mygroup
[            clap::build::app] 	App::unroll_args_in_group:iter: entity=mypath
[            clap::build::app] 	App::unroll_args_in_group:iter: this is an arg
[            clap::build::app] 	App::unroll_args_in_group:iter: entity=myusize
[            clap::build::app] 	App::unroll_args_in_group:iter: this is an arg
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[      clap::parse::validator] 	Validator::missing_required_error: req_args=[]
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_smart_usage
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[mygroup], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={mypath, myusize, mygroup}
[            clap::build::app] 	App::unroll_args_in_group: group=mygroup
[            clap::build::app] 	App::unroll_args_in_group:iter: entity=mypath
[            clap::build::app] 	App::unroll_args_in_group:iter: this is an arg
[            clap::build::app] 	App::unroll_args_in_group:iter: entity=myusize
[            clap::build::app] 	App::unroll_args_in_group:iter: this is an arg
[            clap::build::app] 	App::unroll_args_in_group: group=mygroup
[            clap::build::app] 	App::unroll_args_in_group:iter: entity=mypath
[            clap::build::app] 	App::unroll_args_in_group:iter: this is an arg
[            clap::build::app] 	App::unroll_args_in_group:iter: entity=myusize
[            clap::build::app] 	App::unroll_args_in_group:iter: this is an arg
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=["<--mypath <MYPATH>|--myusize <MYUSIZE>>"]
[            clap::build::app] 	App::color: Color setting...
[            clap::build::app] 	Auto
```

---

_Label `C-bug` added by @malthesr on 2022-02-07 14:46_

---

_Comment by @malthesr on 2022-02-07 14:51_

Aha, did manage to find an [earlier discussion](https://github.com/clap-rs/clap/discussions/3184) after all. Seems the solution is to wrap in option.

I'll close this - apologies for the noise.

---

_Closed by @malthesr on 2022-02-07 14:51_

---
