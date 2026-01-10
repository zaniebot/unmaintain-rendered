---
number: 5809
title: "Optional `#[command(flatten)]` breaks when nesting"
type: issue
state: closed
author: kaplanz
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2024-11-06T20:30:00Z
updated_at: 2024-11-06T21:00:05Z
url: https://github.com/clap-rs/clap/issues/5809
synced_at: 2026-01-10T01:28:17Z
---

# Optional `#[command(flatten)]` breaks when nesting

---

_Issue opened by @kaplanz on 2024-11-06 20:30_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.82.0 (f6e511eec 2024-10-15)

### Clap Version

4.5.20

### Minimal reproducible code

```rust
use clap::{Args, Parser};

fn main() {
    dbg!(Cli::parse());
}

#[derive(Debug, Parser)]
struct Cli {
    #[command(flatten)]
    pub opt: Option<Opt>,
}

#[derive(Args, Debug)]
pub struct Opt {
    #[arg(long)]
    pub bar: bool,

    #[arg(long)]
    pub baz: Option<i32>,

    // NOTE: Everything works if you comment out this field!
    #[command(flatten)]
    pub bad: Oops,
}

#[derive(Args, Debug)]
pub struct Oops {
    #[arg(long)]
    pub oops: bool
}
```

### Steps to reproduce the bug with the above code

Run the produced executable, supplying any args on the command-line within the `opt` struct:

```
$ cargo run -- --bar --baz 3 --oops
```

### Actual Behaviour

The minimum reproducible code will correctly parse the supplied options as shown in the (correct) help text:

```
Usage: foo [OPTIONS]

Options:
      --bar        
      --baz <BAZ>  
      --oops       
  -h, --help       Print help
```

However, as can clearly been seen in the parsed output, the parsed values are seemingly discarded:

```
$ cargo run -- --bar --baz 3 --oops
[src/main.rs:4:5] Cli::parse() = Cli {
    opt: None,
}
```

### Expected Behaviour

What is strange is that this issue **only** occurs with the nested `#[command(flatten)]` on lines 22-23. If this line is removed, the output will be as expected (with the obvious omission of the now-removed `--oops` flag):

```
$ cargo run -- --bar --baz 3
[src/main.rs:4:5] Cli::parse() = Cli {
    opt: Some(
        Opt {
            bar: true,
            baz: Some(
                3,
            ),
        },
    ),
}
```

Comparing the debug output between with/without the problematic lines, there's notably the following line missing:

```
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Opt", source=CommandLine
```

### Additional Context

### Possibly Related

- #3566
  - @epage I believe this is a similar issue to [this comment](https://github.com/clap-rs/clap/discussions/3566#discussioncomment-3860552).
- #4350

### Debug Output

```
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.01s
     Running `foo --bar --baz 3 --oops`
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="foo"
[clap_builder::builder::command]Command::_propagate:foo
[clap_builder::builder::command]Command::_check_help_and_version:foo expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:foo
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:bar
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:baz
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:oops
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--bar"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--bar")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--bar'
[clap_builder::parser::parser]Parser::parse_long_arg("bar"): Presence validated
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Parser::react: has default_missing_vals
[clap_builder::parser::parser]Parser::remove_overrides: id="bar"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="bar", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="bar"
[clap_builder::parser::parser]Parser::push_arg_values: ["true"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::get_matches_with: After parse_long_arg ValuesDone
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--baz"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--baz")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--baz <BAZ>'
[clap_builder::parser::parser]Parser::parse_long_arg("baz"): Found an arg with value 'None'
[clap_builder::parser::parser]Parser::parse_opt_value; arg=baz, val=None, has_eq=false
[clap_builder::parser::parser]Parser::parse_opt_value; arg.settings=ArgFlags(0)
[clap_builder::parser::parser]Parser::parse_opt_value; Checking for val...
[clap_builder::parser::parser]Parser::parse_opt_value: More arg vals required...
[clap_builder::parser::parser]Parser::get_matches_with: After parse_long_arg Opt("baz")
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"3"'
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=baz, pending=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=1
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--oops"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--oops")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--oops'
[clap_builder::parser::parser]Parser::parse_long_arg("oops"): Presence validated
[clap_builder::parser::parser]Parser::resolve_pending: id="baz"
[clap_builder::parser::parser]Parser::react action=Set, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Parser::react: cur_idx:=2
[clap_builder::parser::parser]Parser::remove_overrides: id="baz"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="baz", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="baz"
[clap_builder::parser::parser]Parser::push_arg_values: ["3"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=3
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=baz, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Parser::react: has default_missing_vals
[clap_builder::parser::parser]Parser::remove_overrides: id="oops"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="oops", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="oops"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Oops", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["true"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=4
[clap_builder::parser::parser]Parser::get_matches_with: After parse_long_arg ValuesDone
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:bar:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:bar: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:bar: was used
[clap_builder::parser::parser]Parser::add_defaults:iter:baz:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:baz: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:oops:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:oops: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:oops: was used
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="bar"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="bar", conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="baz"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="baz", conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="oops"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="oops", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="Oops", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"bar"
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"baz"
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"oops"
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"Oops"
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="bar"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="bar"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="baz"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="baz"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="oops"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="oops"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"bar"
[clap_builder::parser::validator]Validator::gather_requires:iter:"baz"
[clap_builder::parser::validator]Validator::gather_requires:iter:"oops"
[clap_builder::parser::validator]Validator::gather_requires:iter:"Oops"
[clap_builder::parser::validator]Validator::gather_requires:iter:"Oops":group
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::arg_matcher]ArgMatcher::get_global_values: global_arg_vec=[]
```

---

_Label `C-bug` added by @kaplanz on 2024-11-06 20:30_

---

_Label `A-derive` added by @epage on 2024-11-06 20:59_

---

_Comment by @epage on 2024-11-06 21:00_

I believe this is a duplicate of #4697, closing in favor of that.  If there is a reason to keep this open separately, let us know!

---

_Closed by @epage on 2024-11-06 21:00_

---
