---
number: 4462
title: "[derive] Exclusive can not be used with required"
type: issue
state: closed
author: fmorency
labels:
  - C-bug
assignees: []
created_at: 2022-11-07T16:17:35Z
updated_at: 2022-11-07T21:50:58Z
url: https://github.com/clap-rs/clap/issues/4462
synced_at: 2026-01-10T01:27:56Z
---

# [derive] Exclusive can not be used with required

---

_Issue opened by @fmorency on 2022-11-07 16:17_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.65.0 (897e37553 2022-11-02)

### Clap Version

4.0.20

### Minimal reproducible code

```rust
use clap::Parser;
use std::path::PathBuf;

#[derive(Parser)]
struct Opts {
    #[arg(long, action, exclusive = true)]
    list_stuff: bool,

    #[arg(long, required = true)]
    input: PathBuf,
}

fn main() {
    let Opts { input, list_stuff } = Opts::parse();
}

```


### Steps to reproduce the bug with the above code

`cargo run -- --list-stuff`

### Actual Behaviour

```
error: The following required argument was not provided: input

Usage: clap_exclusive [OPTIONS] --input <INPUT>

For more information try '--help'
```

### Expected Behaviour

The program should run without error.

### Additional Context

Relates #3595

The Builder behaves correctly

```rust
use clap::{Arg, Command};

fn main() {
    let matches = Command::new("bug")
        .arg(
            Arg::new("list-stuff")
                .long("list-stuff")
                .action(clap::ArgAction::SetTrue)
                .exclusive(true),
        )
        .arg(Arg::new("input").required(true))
        .get_matches();

    if let Some(&list_stuff) = matches.get_one::<bool>("list-stuff") {
        if list_stuff {
            println!("Stuff!");
        }
    }
}
```

### Debug Output

```
[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="clap_exclusive"
[      clap::builder::command] 	Command::_propagate:clap_exclusive
[      clap::builder::command] 	Command::_check_help_and_version:clap_exclusive expand_help_tree=false
[      clap::builder::command] 	Command::long_help_exists
[      clap::builder::command] 	Command::_check_help_and_version: Building default --help
[      clap::builder::command] 	Command::_propagate_global_args:clap_exclusive
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:list_stuff
[clap::builder::debug_asserts] 	Arg::_debug_asserts:input
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--list-stuff")' ([45, 45, 108, 105, 115, 116, 45, 115, 116, 117, 102, 102])
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("--list-stuff")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::parse_long_arg
[        clap::parser::parser] 	Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser] 	Parser::parse_long_arg: Found valid arg or flag '--list-stuff'
[        clap::parser::parser] 	Parser::parse_long_arg("list-stuff"): Presence validated
[        clap::parser::parser] 	Parser::react action=SetTrue, identifier=Some(Long), source=CommandLine
[        clap::parser::parser] 	Parser::react: has default_missing_vals
[        clap::parser::parser] 	Parser::remove_overrides: id="list_stuff"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="list_stuff", source=CommandLine
[      clap::builder::command] 	Command::groups_for_arg: id="list_stuff"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="Opts", source=CommandLine
[        clap::parser::parser] 	Parser::push_arg_values: ["true"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[        clap::parser::parser] 	Parser::get_matches_with: After parse_long_arg ValuesDone
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:list_stuff:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:list_stuff: has default vals
[        clap::parser::parser] 	Parser::add_default_value:iter:list_stuff: was used
[        clap::parser::parser] 	Parser::add_defaults:iter:input:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:input: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[     clap::parser::validator] 	Validator::validate
[     clap::parser::validator] 	Validator::validate_conflicts
[     clap::parser::validator] 	Validator::validate_exclusive
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id="list_stuff"
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg="list_stuff"
[      clap::builder::command] 	Command::groups_for_arg: id="list_stuff"
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id="list_stuff", conflicts=[]
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id="Opts", conflicts=[]
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_required: required=ChildGraph([Child { id: "input", children: [] }])
[     clap::parser::validator] 	Validator::gather_requires
[     clap::parser::validator] 	Validator::gather_requires:iter:"list_stuff"
[     clap::parser::validator] 	Validator::gather_requires:iter:"Opts"
[     clap::parser::validator] 	Validator::gather_requires:iter:"Opts":group
[     clap::parser::validator] 	Validator::validate_required: is_exclusive_present=true
[     clap::parser::validator] 	Validator::validate_required:iter:aog="input"
[     clap::parser::validator] 	Validator::validate_required:iter: This is an arg
[   clap::parser::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[]
[      clap::builder::command] 	Command::_build: name="clap_exclusive"
[      clap::builder::command] 	Command::_propagate:clap_exclusive
[      clap::builder::command] 	Command::_check_help_and_version:clap_exclusive expand_help_tree=false
[      clap::builder::command] 	Command::long_help_exists
[      clap::builder::command] 	Command::_check_help_and_version: Building default --help
[      clap::builder::command] 	Command::_propagate_global_args:clap_exclusive
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:list_stuff
[clap::builder::debug_asserts] 	Arg::_debug_asserts:input
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Command::_verify_positionals
[      clap::builder::command] 	Command::_build: name="clap_exclusive"
[      clap::builder::command] 	Command::_build: already built
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::needs_options_tag
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=list_stuff
[      clap::builder::command] 	Command::groups_for_arg: id="list_stuff"
[         clap::output::usage] 	Usage::needs_options_tag:iter:iter: grp_s="Opts"
[         clap::output::usage] 	Usage::needs_options_tag:iter: [OPTIONS] required
[         clap::output::usage] 	Usage::get_args: incls=[]
[         clap::output::usage] 	Usage::get_args: unrolled_reqs=["input"]
[         clap::output::usage] 	Usage::get_args: ret_val=[StyledStr { pieces: [(Some(Literal), "--"), (Some(Literal), "input"), (Some(Placeholder), " "), (Some(Placeholder), "<INPUT>")] }]
[         clap::output::usage] 	Usage::create_help_usage: usage=clap_exclusive [OPTIONS] --input <INPUT>
[      clap::builder::command] 	Command::color: Color setting...
[      clap::builder::command] 	Auto
[      clap::builder::command] 	Command::color: Color setting...
[      clap::builder::command] 	Auto
```

---

_Label `C-bug` added by @fmorency on 2022-11-07 16:17_

---

_Comment by @fmorency on 2022-11-07 16:24_

This issue is also present in clap `3.2.23`.

```rust
use clap::Parser;
use std::path::PathBuf;

#[derive(Parser)]
struct Opts {
    #[clap(long, action, exclusive = true)]
    list_stuff: bool,

    #[clap(long, required = true)]
    input: PathBuf,
}

fn main() {
    let Opts { input, list_stuff } = Opts::parse();
}
```

---

_Comment by @epage on 2022-11-07 16:43_

When the derive is populating your struct, it has nothing to put in `input`, so it reports an error about a required argument not being present (not that `required = true` is implicit for non-Option fields).

So you need to mark the field with `Option`
```rust
use clap::Parser;
use std::path::PathBuf;

#[derive(Parser)]
struct Opts {
    #[arg(long, action, exclusive = true)]
    list_stuff: bool,

    #[arg(long, required = true)]
    input: Option<PathBuf>,
}

fn main() {
    let Opts { input, list_stuff } = Opts::parse();
}
```

---

_Comment by @fmorency on 2022-11-07 16:54_

Thanks, @epage that makes sense. I wonder if the error message could be more explicit.

---

_Comment by @epage on 2022-11-07 18:57_

We can't catch these errors at compile time, so we have to leave them to runtime.  These errors can only be hit when fully exercising all of an application (e.g. testing every subcommand).

In most cases it will be because of a programming error but not in all, so we can't just panic or show an internal error, which is why we show the closest relevant user-facing error.

I wish I knew of a better way of helping people zero in on this as this is a fairly common problem

Some others that I found from a quick search
- #4427

(I know there are more; just not finding them atm)

---

_Comment by @fmorency on 2022-11-07 21:50_

Thank you for your insight, @epage; this is very useful. I am closing this issue now.

---

_Closed by @fmorency on 2022-11-07 21:50_

---
