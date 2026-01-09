---
number: 5040
title: "`value_terminator` has no effect when it is the first argument"
type: issue
state: open
author: mamekoro
labels:
  - C-bug
  - E-medium
  - ":money_with_wings: $10"
assignees: []
created_at: 2023-07-22T15:39:33Z
updated_at: 2024-06-26T21:17:26Z
url: https://github.com/clap-rs/clap/issues/5040
synced_at: 2026-01-07T13:12:20-06:00
---

# `value_terminator` has no effect when it is the first argument

---

_Issue opened by @mamekoro on 2023-07-22 15:39_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.71.0 (8ede3aae2 2023-07-12)

### Clap Version

4.3.19

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Clone, Debug, Parser)]
pub struct Args {
    #[arg(allow_hyphen_values = true, value_terminator = "--")]
    pub opts: Vec<String>,

    #[arg(allow_hyphen_values = true)]
    pub cmdline: Vec<String>,
}

fn main() {
    let args = Args::parse_from(["program", "--", "ls", "-l"]);
    println!("opts = {:?}, cmdline = {:?}", args.opts, args.cmdline);
}
```

`opts` is options for the program itself, and `cmdline` is arguments passed to an external program.

### Steps to reproduce the bug with the above code

```shell
cargo add clap --features derive
cargo run
```

### Actual Behaviour

It prints `opts = ["ls", "-l"], cmdline = []`

### Expected Behaviour

It should print `opts = [], cmdline = ["ls", "-l"]`

### Additional Context

This is related to https://github.com/clap-rs/clap/discussions/4960 and https://github.com/clap-rs/clap/pull/5037.

In addition to the reproduction above, I also tested the following code, all of them worked correctly.

`Args::parse_from(["program", "--name", "xyz", "--", "ls", "-l"])`
prints `opts = ["--name", "xyz"], cmdline = ["ls", "-l"]`

`Args::parse_from(["program", "--name", "xyz", "--"])`
prints `opts = ["--name", "xyz"], cmdline = []`

`Args::parse_from(["program", "--name", "xyz"])`
prints `opts = ["--name", "xyz"], cmdline = []`

`Args::parse_from(["program", "--"])`
prints `opts = [], cmdline = []`

`Args::parse_from(["program"])`
prints `opts = [], cmdline = []`


### Debug Output

<details>

```text
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="test-clap-hyphen"
[clap_builder::builder::command]Command::_propagate:test-clap-hyphen
[clap_builder::builder::command]Command::_check_help_and_version:test-clap-hyphen expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:test-clap-hyphen
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:opts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:cmdline
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: setting TrailingVals=true
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"ls"'
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...true
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-l"'
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...true
[clap_builder::parser::parser]Parser::resolve_pending: id="opts"
[clap_builder::parser::parser]Parser::react action=Append, identifier=Some(Index), source=CommandLine
[clap_builder::parser::parser]Parser::remove_overrides: id="opts"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="opts", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="opts"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Args", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["ls", "-l"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=opts, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1..=18446744073709551615, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:opts:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:opts: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:cmdline:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:cmdline: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="opts"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="opts", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="Args", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="opts"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="opts"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"opts"
[clap_builder::parser::validator]Validator::gather_requires:iter:"Args"
[clap_builder::parser::validator]Validator::gather_requires:iter:"Args":group
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::arg_matcher]ArgMatcher::get_global_values: global_arg_vec=[]
opts = ["ls", "-l"], cmdline = []
```

</details>

---

_Label `C-bug` added by @mamekoro on 2023-07-22 15:39_

---

_Label `E-medium` added by @epage on 2023-07-24 19:31_

---

_Comment by @epage on 2023-07-24 19:31_

Huh, I know I tested for this case.  I guess I didn't pay close enough attention to the result

---

_Label `:money_with_wings: $10` added by @pksunkara on 2024-03-26 00:39_

---
