```yaml
number: 4569
title: Required field inside a struct that conflicts with another arg is still demanded 
type: issue
state: closed
author: nyurik
labels:
  - C-bug
  - E-medium
  - A-validators
assignees: []
created_at: 2022-12-22T04:01:26Z
updated_at: 2022-12-22T19:19:47Z
url: https://github.com/clap-rs/clap/issues/4569
synced_at: 2026-01-12T16:14:16Z
```

# Required field inside a struct that conflicts with another arg is still demanded 

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
use std::path::PathBuf;
use clap::Parser;

#[derive(Parser, Debug, PartialEq, Default)]
#[command(about, version)]
pub struct Args {
    #[arg(short, long, conflicts_with = "ExplicitConfig")]
    config: Option<PathBuf>,
    #[command(flatten)]
    explicit: Option<ExplicitConfig>,
}

#[derive(clap::Args, Debug, PartialEq, Default)]
#[command(about, version)]
pub struct ExplicitConfig {
    #[arg(required = true)]
    connection: Vec<String>,
}

fn main() {
    let args = Args::parse_from(["main", "--config", "foo.toml"]);
    let expected = Args {
        config: Some(PathBuf::from("foo.toml")),
        ..Default::default()
    };
    assert_eq!(args, expected);
}
```


### Steps to reproduce the bug with the above code

run it

### Actual Behaviour

```
error: The following required arguments were not provided:
  <CONNECTION>...

Usage: main --config <CONFIG> <CONNECTION>...

For more information try '--help'

Process finished with exit code 2
```

### Expected Behaviour

should pass

### Additional Context

Filing per @epage suggestion in a [discussion](https://github.com/clap-rs/clap/discussions/4562#discussioncomment-4453976).

### Debug Output

```
/home/nyurik/.cargo/bin/cargo run --color=always --package tmp --bin tmp
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
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
[clap::builder::debug_asserts] 	Arg::_debug_asserts:config
[clap::builder::debug_asserts] 	Arg::_debug_asserts:connection
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Arg::_debug_asserts:version
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--config")' ([45, 45, 99, 111, 110, 102, 105, 103])
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("--config")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::parse_long_arg
[        clap::parser::parser] 	Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser] 	Parser::parse_long_arg: Found valid arg or flag '--config <CONFIG>'
[        clap::parser::parser] 	Parser::parse_long_arg("config"): Found an arg with value 'None'
[        clap::parser::parser] 	Parser::parse_opt_value; arg=config, val=None, has_eq=false
[        clap::parser::parser] 	Parser::parse_opt_value; arg.settings=ArgFlags(NO_OP)
[        clap::parser::parser] 	Parser::parse_opt_value; Checking for val...
[        clap::parser::parser] 	Parser::parse_opt_value: More arg vals required...
[        clap::parser::parser] 	Parser::get_matches_with: After parse_long_arg Opt("config")
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("foo.toml")' ([102, 111, 111, 46, 116, 111, 109, 108])
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=config, pending=1
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: expected=1, actual=1
[        clap::parser::parser] 	Parser::resolve_pending: id="config"
[        clap::parser::parser] 	Parser::react action=Set, identifier=Some(Long), source=CommandLine
[        clap::parser::parser] 	Parser::react: cur_idx:=1
[        clap::parser::parser] 	Parser::remove_overrides: id="config"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="config", source=CommandLine
[      clap::builder::command] 	Command::groups_for_arg: id="config"
[        clap::parser::parser] 	Parser::push_arg_values: ["foo.toml"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=2
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=config, pending=0
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: expected=1, actual=0
[        clap::parser::parser] 	Parser::react not enough values passed in, leaving it to the validator to complain
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:config:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:config: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:connection:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:connection: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:version:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:version: doesn't have default vals
[     clap::parser::validator] 	Validator::validate
[     clap::parser::validator] 	Validator::validate_conflicts
[     clap::parser::validator] 	Validator::validate_exclusive
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id="config"
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg="config"
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_required: required=ChildGraph([Child { id: "connection", children: [] }])
[     clap::parser::validator] 	Validator::gather_requires
[     clap::parser::validator] 	Validator::gather_requires:iter:"config"
[     clap::parser::validator] 	Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator] 	Validator::validate_required:iter:aog="connection"
[     clap::parser::validator] 	Validator::validate_required:iter: This is an arg
[     clap::parser::validator] 	Validator::is_missing_required_ok: connection
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg="connection"
[      clap::builder::command] 	Command::groups_for_arg: id="connection"
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id="connection", conflicts=[]
[      clap::builder::command] 	Command::groups_for_arg: id="config"
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id="config", conflicts=["ExplicitConfig"]
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_required:iter: Missing "connection"
[     clap::parser::validator] 	Validator::missing_required_error; incl=["connection"]
[     clap::parser::validator] 	Validator::missing_required_error: reqs=ChildGraph([Child { id: "connection", children: [] }])
[         clap::output::usage] 	Usage::get_required_usage_from: incls=["connection"], matcher=true, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs=["connection"]
[         clap::output::usage] 	Usage::get_required_usage_from:iter:"connection" arg is_present=false
[         clap::output::usage] 	Usage::get_required_usage_from:iter:"connection" arg is_present=false
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[StyledStr { pieces: [(Some(Placeholder), "<CONNECTION>...")] }]
[     clap::parser::validator] 	Validator::missing_required_error: req_args=[
    "<CONNECTION>...",
]
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_smart_usage
[         clap::output::usage] 	Usage::get_args: incls=["config", "connection"]
[         clap::output::usage] 	Usage::get_args: unrolled_reqs=["connection"]
[         clap::output::usage] 	Usage::get_args: ret_val=[StyledStr { pieces: [(Some(Literal), "--"), (Some(Literal), "config"), (Some(Placeholder), " "), (Some(Placeholder), "<CONFIG>")] }, StyledStr { pieces: [(Some(Placeholder), "<CONNECTION>...")] }]
[      clap::builder::command] 	Command::color: Color setting...
[      clap::builder::command] 	Auto
[      clap::builder::command] 	Command::color: Color setting...
[      clap::builder::command] 	Auto
error: The following required arguments were not provided:
  <CONNECTION>...

Usage: main --config <CONFIG> <CONNECTION>...

For more information try '--help'

Process finished with exit code 2
```

---

_Label `C-bug` added by @nyurik on 2022-12-22 04:01_

---

_Label `E-medium` added by @epage on 2022-12-22 14:54_

---

_Label `A-validators` added by @epage on 2022-12-22 14:54_

---

_Comment by @epage on 2022-12-22 15:07_

If we supported `#[group)]` attribute (#3165), a workaround would be to do `#[group(conflicts_with = "config")]`.  Instead, the workaround would be to define your own group like `#[command(group = ArgGroup::new("non-config").arg("connection").conflicts_with("config"))]`.

---

_Comment by @epage on 2022-12-22 15:13_

When we normally process conflicts, the "ExplicitConfig" will be present and it works (e.g. if you run `cargo run -- connection`).

The problem is when we use conflicts for detection when a missing required is ok.  We check "connection"s conflicts are not present and "config"s conflicts are not present.  In this case that is "ExplicitConfig" which won't be present.  We also need to check the group.

---

_Referenced in [clap-rs/clap#4573](../../clap-rs/clap/pulls/4573.md) on 2022-12-22 19:09_

---

_Closed by @epage on 2022-12-22 19:19_

---
