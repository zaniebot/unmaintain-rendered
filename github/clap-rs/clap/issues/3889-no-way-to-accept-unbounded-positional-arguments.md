---
number: 3889
title: No way to accept unbounded positional arguments with subcommand
type: issue
state: closed
author: nissaofthesea
labels:
  - C-bug
assignees: []
created_at: 2022-06-30T19:36:34Z
updated_at: 2022-09-28T16:19:24Z
url: https://github.com/clap-rs/clap/issues/3889
synced_at: 2026-01-07T13:12:20-06:00
---

# No way to accept unbounded positional arguments with subcommand

---

_Issue opened by @nissaofthesea on 2022-06-30 19:36_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.61.0 (fe5b13d68 2022-05-18)

### Clap Version

3.2.8

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Parser, Debug)]
#[clap(subcommand_required = true)]
struct Args {
    #[clap()]
    names: Vec<String>,

    #[clap(subcommand)]
    action: Action,
}

#[derive(Subcommand, Debug)]
enum Action {
    Greet,
}

fn main() {
    for name in Args::parse().names {
        println!("hey {}", name);
    }
}
```


### Steps to reproduce the bug with the above code

1. See help message

```console
$ cargo r -- -h
cli

USAGE:
    cli [NAMES]... <SUBCOMMAND>

ARGS:
    <NAMES>...    

OPTIONS:
    -h, --help    Print help information

SUBCOMMANDS:
    greet    
    help     Print this message or the help of the given subcommand(s)
```

2. Run program according to help message

```console
$ cargo r -- name1 greet
```


### Actual Behaviour

When I run the program according to the help message usage, it returns an error

```console
$ cargo r -- name1 greet
error: 'cli' requires a subcommand but one was not provided

USAGE:
    cli [NAMES]... <SUBCOMMAND>

For more information try --help
```


### Expected Behaviour

The syntax given by the help message should work, or the help message should give a valid syntax.

```console
$ cargo r -- name1 greet
hey name1
```

Or, there should be an assertion somewhere about this specific configuration, since it also doesn't work to pass the names after the subcommand (there is no way to pass names).


### Additional Context

Related: https://github.com/clap-rs/clap/issues/3721

I'm able to pass names when `names` is global, but like is mentioned in the related issue, the help message shows the order differently than is valid.

Apologies if this is the same issue as #3721.


### Debug Output

```
[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="cli"
[      clap::builder::command] 	Command::_propagate:cli
[      clap::builder::command] 	Command::_check_help_and_version: cli
[      clap::builder::command] 	Command::_check_help_and_version: Removing generated version
[      clap::builder::command] 	Command::_check_help_and_version: Building help subcommand
[      clap::builder::command] 	Command::_propagate_global_args:cli
[      clap::builder::command] 	Command::_propagate removing greet's help
[      clap::builder::command] 	Command::_propagate pushing help to greet
[      clap::builder::command] 	Command::_propagate pushing names to greet
[      clap::builder::command] 	Command::_propagate removing help's help
[      clap::builder::command] 	Command::_propagate pushing help to help
[      clap::builder::command] 	Command::_propagate pushing names to help
[      clap::builder::command] 	Command::_derive_display_order:cli
[      clap::builder::command] 	Command::_derive_display_order:greet
[      clap::builder::command] 	Command::_derive_display_order:help
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Arg::_debug_asserts:names
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("name1")' ([110, 97, 109, 101, 49])
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...1
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("name1")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::split_arg_values; arg=names, val=RawOsStr("name1")
[        clap::parser::parser] 	Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("greet")' ([103, 114, 101, 101, 116])
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...1
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser] 	Parser::resolve_pending: id=names
[        clap::parser::parser] 	Parser::react action=StoreValue, identifier=Some(Index), source=CommandLine
[        clap::parser::parser] 	Parser::remove_overrides: id=names
[   clap::parser::arg_matcher] 	ArgMatcher::start_occurrence_of_arg: id=names
[      clap::builder::command] 	Command::groups_for_arg: id=names
[        clap::parser::parser] 	Parser::push_arg_values: ["name1"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[      clap::builder::command] 	Command::groups_for_arg: id=names
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=names, resolved=1, pending=0
[        clap::parser::parser] 	Parser::split_arg_values; arg=names, val=RawOsStr("greet")
[        clap::parser::parser] 	Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[        clap::parser::parser] 	Parser::resolve_pending: id=names
[        clap::parser::parser] 	Parser::react action=StoreValue, identifier=Some(Index), source=CommandLine
[        clap::parser::parser] 	Parser::remove_overrides: id=names
[   clap::parser::arg_matcher] 	ArgMatcher::start_occurrence_of_arg: id=names
[      clap::builder::command] 	Command::groups_for_arg: id=names
[        clap::parser::parser] 	Parser::push_arg_values: ["greet"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=2
[      clap::builder::command] 	Command::groups_for_arg: id=names
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=names, resolved=2, pending=0
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:names:
[        clap::parser::parser] 	Parser::add_default_value:iter:names: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:names: doesn't have default vals
[     clap::parser::validator] 	Validator::validate
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val={}
[         clap::output::usage] 	Usage::needs_options_tag
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=help
[         clap::output::usage] 	Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage] 	Usage::needs_options_tag: [OPTIONS] not required
[         clap::output::usage] 	Usage::get_args_tag; incl_reqs = true
[         clap::output::usage] 	Usage::get_args_tag:iter:names
[      clap::builder::command] 	Command::groups_for_arg: id=names
[         clap::output::usage] 	Usage::get_args_tag:iter: 1 Args not required or hidden
[      clap::builder::command] 	Command::groups_for_arg: id=names
[         clap::output::usage] 	Usage::get_args_tag:iter: Exactly one, returning 'names'
[          clap::builder::arg] 	Arg::name_no_brackets:names
[          clap::builder::arg] 	Arg::name_no_brackets: val_names=[
    "NAMES",
]
[         clap::output::usage] 	Usage::create_help_usage: usage=cli [NAMES]... <SUBCOMMAND>
[      clap::builder::command] 	Command::color: Color setting...
[      clap::builder::command] 	Auto
```


---

_Label `C-bug` added by @nissaofthesea on 2022-06-30 19:36_

---

_Comment by @epage on 2022-07-02 02:13_

> The syntax given by the help message should work, or the help message should give a valid syntax.

This requires the user doing this intentionally and designing their CLI so that a valid positional value never conflicts with a subcommand and to never use `external_subcommand`

> Or, there should be an assertion somewhere about this specific configuration, since it also doesn't work to pass the names after the subcommand (there is no way to pass names).

This is describing https://github.com/clap-rs/clap/issues/3721

If you feel this is an acceptable resolution, I might close this as a duplicate of that.

---

_Comment by @nissaofthesea on 2022-07-02 04:37_

Mhm, having an assertion for this specific case would be a clear way to enforce that as a way not to design a cli.
With that/when assertion is added I think this issue is resolved.

#3721 is a little different because the positional argument there is global.
For that issue, I do think the help message should be different; I'm not sure I followed your comment there. Using the format given by the help message doesn't actually work, you have to put the positional argument at the end.


---

_Comment by @epage on 2022-07-11 20:25_

In #3721, the bug is that we allowed unbound positional arguments at the same level as subcommands; that is unambiguous and we can't really do much about it.  This is why I was suggesting we prevent users from even doing this, like here.  This is independent of `global(true)`.

---

_Comment by @epage on 2022-09-28 16:19_

Closing this in favor of #3721 

---

_Closed by @epage on 2022-09-28 16:19_

---
