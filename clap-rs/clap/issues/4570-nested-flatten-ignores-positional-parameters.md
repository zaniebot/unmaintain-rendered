```yaml
number: 4570
title: Nested flatten ignores positional parameters
type: issue
state: closed
author: nyurik
labels:
  - C-bug
  - A-parsing
  - E-medium
assignees: []
created_at: 2022-12-22T04:25:16Z
updated_at: 2023-02-10T15:25:35Z
url: https://github.com/clap-rs/clap/issues/4570
synced_at: 2026-01-12T16:14:16Z
```

# Nested flatten ignores positional parameters

---

_@nyurik_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.66.0 (69f9c33d7 2022-12-12)

### Clap Version

4.0.30

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser, Debug, PartialEq, Default)]
#[command(about, version)]
pub struct Args {
    #[command(flatten)]
    val1: Option<SubArgs>,
}

#[derive(clap::Args, Debug, PartialEq, Default)]
#[command(about, version)]
pub struct SubArgs {
    val2: Vec<String>,
    #[command(flatten)]
    sub_val2: SubSubArgs,
}

#[derive(clap::Args, Debug, PartialEq, Default)]
#[command(about, version)]
pub struct SubSubArgs {
    #[arg(short, long)]
    pub val3: Option<i32>,
}

fn main() {
    let args = Args::parse_from(["main", "bar"]);
    let expected = Args {
        val1: Some(SubArgs {
            val2: vec!["bar".to_string()],
            sub_val2: SubSubArgs {
                val3: None
            },
        }),
    };
    assert_eq!(args, expected);
}
```


### Steps to reproduce the bug with the above code

run it

### Actual Behaviour

parse succeeds, but ignores the first positional parameter.

### Expected Behaviour

first positional parameter should be added to the `val2` Vec.

### Additional Context

_No response_

### Debug Output

```
/home/nyurik/.cargo/bin/cargo run --color=always --package tmp --bin tmp
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/tmp`
[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="tmp"
[      clap::builder::command] 	Command::_propagate:tmp
[      clap::builder::command] 	Command::_check_help_and_version:tmp expand_help_tree=false
[      clap::builder::command] 	Command::long_help_exists
[      clap::builder::command] 	Command::_check_help_and_version: Building default --help
[      clap::builder::command] 	Command::_check_help_and_version: Building default --version
[      clap::builder::command] 	Command::_propagate_global_args:tmp
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:val2
[clap::builder::debug_asserts] 	Arg::_debug_asserts:val3
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Arg::_debug_asserts:version
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("bar")' ([98, 97, 114])
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("bar")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...1
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser] 	Parser::resolve_pending: id="val2"
[        clap::parser::parser] 	Parser::react action=Append, identifier=Some(Index), source=CommandLine
[        clap::parser::parser] 	Parser::remove_overrides: id="val2"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="val2", source=CommandLine
[      clap::builder::command] 	Command::groups_for_arg: id="val2"
[        clap::parser::parser] 	Parser::push_arg_values: ["bar"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=val2, pending=0
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: expected=1..=18446744073709551615, actual=0
[        clap::parser::parser] 	Parser::react not enough values passed in, leaving it to the validator to complain
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:val2:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:val2: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:val3:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:val3: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:version:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:version: doesn't have default vals
[     clap::parser::validator] 	Validator::validate
[     clap::parser::validator] 	Validator::validate_conflicts
[     clap::parser::validator] 	Validator::validate_exclusive
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id="val2"
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg="val2"
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator] 	Validator::gather_requires
[     clap::parser::validator] 	Validator::gather_requires:iter:"val2"
[     clap::parser::validator] 	Validator::validate_required: is_exclusive_present=false
[   clap::parser::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[]
thread 'main' panicked at 'assertion failed: `(left == right)`
  left: `Args { val1: None }`,
 right: `Args { val1: Some(SubArgs { val2: ["bar"], sub_val2: SubSubArgs { val3: None } }) }`', src/main.rs:35:5
stack backtrace:
   0: rust_begin_unwind
             at /rustc/69f9c33d71c871fc16ac445211281c6e7a340943/library/std/src/panicking.rs:575:5
   1: core::panicking::panic_fmt
             at /rustc/69f9c33d71c871fc16ac445211281c6e7a340943/library/core/src/panicking.rs:65:14
   2: core::panicking::assert_failed_inner
   3: core::panicking::assert_failed
             at /rustc/69f9c33d71c871fc16ac445211281c6e7a340943/library/core/src/panicking.rs:203:5
   4: tmp::main
             at ./src/main.rs:35:5
   5: core::ops::function::FnOnce::call_once
             at /rustc/69f9c33d71c871fc16ac445211281c6e7a340943/library/core/src/ops/function.rs:251:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

Process finished with exit code 101
```

---

_Label `C-bug` added by @nyurik on 2022-12-22 04:25_

---

_Comment by @epage on 2022-12-22 14:51_

This was broken by #4350 because we aren't correctly processing nested groups, so when we do `contains_id("Args")`, it returns false and we don't process any of the arguments.

---

_Label `A-parsing` added by @epage on 2022-12-22 14:51_

---

_Label `E-medium` added by @epage on 2022-12-22 14:51_

---

_Comment by @epage on 2022-12-22 18:48_

So looking at this further, this is specifically when using `Option` with `flatten` and only because we haven't implemented nesting of groups.  Because "SubArgs" has a group within it, we just don't bother populating the group.

---

_Comment by @nyurik on 2022-12-23 22:48_

Thanks for the feedback. The biggest issue with this bug is that it silently ignores input, making it harder to even notice it

---

_Comment by @epage on 2023-02-10 15:25_

Huh, for some reason I hadn't linked to this from the tracking issue so when #4697 came along, I linked that.  I'll go ahead and close in favor of #4697 then.

---

_Closed by @epage on 2023-02-10 15:25_

---
