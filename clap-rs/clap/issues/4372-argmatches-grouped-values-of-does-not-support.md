```yaml
number: 4372
title: "`ArgMatches.grouped_values_of()` does not support groups"
type: issue
state: open
author: etemesi254
labels:
  - C-bug
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2022-10-11T19:32:58Z
updated_at: 2022-11-08T04:30:08Z
url: https://github.com/clap-rs/clap/issues/4372
synced_at: 2026-01-12T16:14:15Z
```

# `ArgMatches.grouped_values_of()` does not support groups

---

_@etemesi254_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.65.0-nightly (20ffea693 2022-08-11)

### Clap Version

4.0.13

### Minimal reproducible code

```rust
use clap::{Arg, ArgAction, Command};

fn build_cli() -> Command {
    Command::new("myprog")
        .arg(Arg::new("exec")
            .short('x')
            .group("gf")
            .action(ArgAction::Append)
        ).arg(Arg::new("vr")
            .short('f')
            .action(ArgAction::Append)
            .group("gf")
    )
}
#[test]
fn test_cli(){
    build_cli().debug_assert();
}
fn main() {
    let m = build_cli()
        .get_matches_from(vec![
            "myprog", "-x", "=g"]);
    let vals: Vec<Vec<&str>> = m.grouped_values_of("gf").unwrap().collect();
    println!("{:?}", vals);
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

thread 'main' panicked at 'Must use `_os` lookups with `Arg::allow_invalid_utf8`', /rustc/20ffea6938b5839c390252e07940b99e3b6a889a/library/core/src/ops/function.rs:164:5

### Expected Behaviour

- It should return argument + value passed.

I might have mis-understood the docs but it does mention  in the panic section and quoting

```text
Panics
If the value is invalid UTF-8.
If id is not a valid argument or group id.
```

Hence I assume it should work with groups.

It does actually parse groups i.e   
```rust
if let Some(counts) = args.grouped_values_of("group_counts") {     
            println!("{:?}",counts.len());
           // works
            for op in operations{
}
```
Prints actual number of arguments for that group but the panic comes when looping, and it corresponds to 

```
#[cfg(feature = "unstable-grouped")]
#[cfg_attr(debug_assertions, track_caller)]
#[inline]
fn unwrap_string(value: &AnyValue) -> &str {
    match value.downcast_ref::<String>() {
        Some(value) => value,
        None => {
            panic!("Must use `_os` lookups with `Arg::allow_invalid_utf8`",)
        }
    }
}
```
Part of the code

### Additional Context

Also `ArgMatches::grouped_values_of` doesn't work with `Action::SetTrue` arguments.




Thanks for the library btw 

### Debug Output

[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="myprog"
[      clap::builder::command] 	Command::_propagate:myprog
[      clap::builder::command] 	Command::_check_help_and_version:myprog expand_help_tree=false
[      clap::builder::command] 	Command::long_help_exists
[      clap::builder::command] 	Command::_check_help_and_version: Building default --help
[      clap::builder::command] 	Command::_propagate_global_args:myprog
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:exec
[clap::builder::debug_asserts] 	Arg::_debug_asserts:vr
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("-x")' ([45, 120])
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("-x")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("x"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['x']) }, invalid_suffix: None }
[        clap::parser::parser] 	Parser::parse_short_arg:iter:x
[        clap::parser::parser] 	Parser::parse_short_arg:iter:x: Found valid opt or flag
[        clap::parser::parser] 	Parser::react action=SetTrue, identifier=Some(Short), source=CommandLine
[        clap::parser::parser] 	Parser::react: has default_missing_vals
[        clap::parser::parser] 	Parser::remove_overrides: id="exec"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="exec", source=CommandLine
[      clap::builder::command] 	Command::groups_for_arg: id="exec"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="gf", source=CommandLine
[        clap::parser::parser] 	Parser::push_arg_values: ["true"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[      clap::builder::command] 	Command::groups_for_arg: id="exec"
[        clap::parser::parser] 	Parser::get_matches_with: After parse_short_arg ValuesDone
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:exec:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:exec: has default vals
[        clap::parser::parser] 	Parser::add_default_value:iter:exec: was used
[        clap::parser::parser] 	Parser::add_defaults:iter:vr:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:vr: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[     clap::parser::validator] 	Validator::validate
[     clap::parser::validator] 	Validator::validate_conflicts
[     clap::parser::validator] 	Validator::validate_exclusive
[     clap::parser::validator] 	Validator::validate_exclusive:iter:"exec"
[     clap::parser::validator] 	Validator::validate_exclusive:iter:"gf"
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id="exec"
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg="exec"
[      clap::builder::command] 	Command::groups_for_arg: id="exec"
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id="exec", conflicts=[]
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id="gf", conflicts=[]
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator] 	Validator::gather_requires
[     clap::parser::validator] 	Validator::gather_requires:iter:"exec"
[     clap::parser::validator] 	Validator::gather_requires:iter:"gf"
[     clap::parser::validator] 	Validator::gather_requires:iter:"gf":group
[     clap::parser::validator] 	Validator::validate_required: is_exclusive_present=false
[   clap::parser::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[]

---

_Label `C-bug` added by @etemesi254 on 2022-10-11 19:32_

---

_Referenced in [clap-rs/clap#2924](../../clap-rs/clap/issues/2924.md) on 2022-10-11 19:43_

---

_Comment by @epage on 2022-10-11 19:49_

`grouped_values_of` has not been updated to the new clap v4 design, meaning it can only return `&str` and not any other type.  The error message is off because these old code paths only dealt with `String` vs `OsString` but with the value parser API, args can be of a variety of types.

I've now linked this from the tracking issue as before this was just a check mark and now we have a specific issue for it

> It should return argument + value passed.

In case there is some confusion, I'm going to step back a bit.

`ArgGroup` allows defining relationships between arguments.  When looking up an `ArgGroup` in `ArgMatches`, it returns which member of the group was matched (as of v4, v3 it returned the values).

Grouped values allows callers to know what sets of values were grouped in an occurrence (`--flag 1 2 3 --flag 4 5 6` would be grouped as `[[1, 2, 3], [4, 5, 6]]`).

Looking up the grouped values of an `ArgGroup` has no meaning as we only track one `ArgGroup` member per group / occurrence.

---

_Comment by @etemesi254 on 2022-10-12 05:22_

I could take a stab at the tracking issue over the course of the oncoming weekends if that's okay with you.

My use case was as follows.

I have a set of command line arguments,  and I need to operations as they were  specified in the command line (order of operations matters),  the example linked in `grouped_value_of` seemed to do that but with positional  arguments(also you pointed it out above) and I did think it the same behaviour would be extended to groups

---

_Comment by @epage on 2022-10-12 11:58_

> I could take a stab at the tracking issue over the course of the oncoming weekends if that's okay with you.

If you'd like, sure

> I have a set of command line arguments, and I need to operations as they were specified in the command line (order of operations matters), 

If you haven't noticed, we have an example for this use case:  https://docs.rs/clap/latest/clap/_derive/_cookbook/find/index.html

>  I did think it the same behaviour would be extended to groups

The challenge is we'd be dealing with multiple argument types.  I've been avoiding exposing support for mixed types as that was a API design / usability line I'm cautious about crossing.

---

_Comment by @etemesi254 on 2022-10-19 04:53_

> If you haven't noticed, we have an example for this use case: https://docs.rs/clap/latest/clap/_derive/_cookbook/find/index.html

This fixes it for me.

Also I was wondering why the grouped_values_of() would look like for groups,

As you mentioned groups may have different types hence is the best choice to disable them completely?


---

_Comment by @epage on 2022-10-19 13:55_

> Also I was wondering why the grouped_values_of() would look like for groups,
> 
> As you mentioned groups may have different types hence is the best choice to disable them completely?

I lean towards leaving the code agnostic to it.  You can look up occurrences of a group but it naively looks it up in our data model which is just the same as doing the non-occurrence look up.

---

_Label `A-parsing` added by @epage on 2022-11-08 04:30_

---

_Label `S-waiting-on-design` added by @epage on 2022-11-08 04:30_

---
