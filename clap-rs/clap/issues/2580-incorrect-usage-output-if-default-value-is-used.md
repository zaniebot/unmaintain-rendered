```yaml
number: 2580
title: Incorrect usage output if default_value is used
type: issue
state: closed
author: fgsch
labels:
  - C-bug
  - A-help
  - ":money_with_wings: $10"
assignees: []
created_at: 2021-07-13T12:22:30Z
updated_at: 2021-07-25T13:20:29Z
url: https://github.com/clap-rs/clap/issues/2580
synced_at: 2026-01-12T16:14:13Z
```

# Incorrect usage output if default_value is used

---

_@fgsch_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.53.0 (53cb7b09b 2021-06-17)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

```rust
use clap::{App, Arg};

fn main() {
    let _ = App::new("bug")
        .arg(
            Arg::new("foo")
                .long("config")
                .takes_value(true)
                .default_value("bar"),
        )
        .arg(Arg::new("input").required(true))
        .get_matches();
}
```

### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

Current code displays:

```
error: The following required arguments were not provided:
    <input>

USAGE:
    baz <input> --config <foo>

For more information try --help
```

### Expected Behaviour

It should display:

```
error: The following required arguments were not provided:
    <input>

USAGE:
    baz [OPTIONS] <input>

For more information try --help
```

### Additional Context

_No response_

### Debug Output

[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:bug
[            clap::build::app] 	App::_derive_display_order:bug
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[clap::build::app::debug_asserts] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:foo
[            clap::build::arg] 	Arg::_debug_asserts:input
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::remove_overrides
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:foo:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:foo: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:foo: wasn't used
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=foo, val="bar"
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."bar"
[            clap::build::app] 	App::groups_for_arg: id=foo
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=foo
[         clap::parse::parser] 	Parser::add_defaults:iter:input:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:input: doesn't have default vals
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::validate_exclusive:iter:foo
[      clap::parse::validator] 	Validator::gather_conflicts
[      clap::parse::validator] 	Validator::gather_conflicts:iter: id=foo
[      clap::parse::validator] 	Validator::gather_conflicts:iter: This is default value, skipping.
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([Child { id: input, children: [] }])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::gather_requirements:iter:foo
[      clap::parse::validator] 	Validator::validate_required:iter:aog=input
[      clap::parse::validator] 	Validator::validate_required:iter: This is an arg
[      clap::parse::validator] 	Validator::is_missing_required_ok: input
[      clap::parse::validator] 	Validator::validate_arg_conflicts: a="input"
[      clap::parse::validator] 	Validator::missing_required_error; incl=None
[      clap::parse::validator] 	Validator::missing_required_error: reqs=ChildGraph([Child { id: input, children: [] }])
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=true, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs=[input]
[         clap::output::usage] 	Usage::get_required_usage_from:iter:input
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=["<input>"]
[      clap::parse::validator] 	Validator::missing_required_error: req_args=[
    "<input>",
]
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_smart_usage
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[foo], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs=[input]
[         clap::output::usage] 	Usage::get_required_usage_from:iter:input
[         clap::output::usage] 	Usage::get_required_usage_from:iter:foo
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=["<input>", "--config <foo>"]
[            clap::build::app] 	App::color: Color setting...
[            clap::build::app] 	Auto
[           clap::output::fmt] 	is_a_tty: stderr=true

---

_Label `T: bug` added by @fgsch on 2021-07-13 12:22_

---

_Label `C: help message` added by @pksunkara on 2021-07-13 14:05_

---

_Label `:money_with_wings: $10` added by @pksunkara on 2021-07-14 20:30_

---

_Comment by @ldm0 on 2021-07-20 18:51_

Nice catch! 👍 

---

_Referenced in [clap-rs/clap#2609](../../clap-rs/clap/pulls/2609.md) on 2021-07-20 18:51_

---

_Closed by @pksunkara on 2021-07-25 13:20_

---
