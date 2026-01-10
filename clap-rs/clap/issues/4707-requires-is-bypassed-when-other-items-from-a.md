---
number: 4707
title: "`requires` is bypassed when other items from a mutually-exclusive `group` are present"
type: issue
state: open
author: mkeeter
labels:
  - C-bug
  - A-validators
  - S-blocked
assignees: []
created_at: 2023-02-13T16:49:49Z
updated_at: 2025-07-03T16:07:50Z
url: https://github.com/clap-rs/clap/issues/4707
synced_at: 2026-01-10T01:28:00Z
---

# `requires` is bypassed when other items from a mutually-exclusive `group` are present

---

_Issue opened by @mkeeter on 2023-02-13 16:49_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.65.0 (897e37553 2022-11-02)

### Clap Version

4.1.4

### Minimal reproducible code

```rust
use clap::{ArgGroup, Parser};

#[derive(Parser, Debug)]
#[clap(group = ArgGroup::new("command").multiple(false))]
struct Args {
    #[clap(long, group = "command")]
    read: bool,

    #[clap(long, group = "command")]
    write: bool,

    #[clap(long, requires = "read")]
    show_hex: bool,
}

fn main() {
    let args = Args::parse();
    println!("{args:?}");
}
```


### Steps to reproduce the bug with the above code

```
cargo run -- --write --show-hex
```

### Actual Behaviour

This command runs without an error.  This is surprising, because `--show-hex` is annotated with `requires = "read"`, and we did not provide the `--read` argument.

### Expected Behaviour

Clap should print an error indicating that the required argument `--read` is not present, e.g.
```
error: the following required arguments were not provided:
  --read

Usage: clap-test --read --show-hex

For more information, try '--help'.
```

### Additional Context

If I remove either `read` or `write` from the command group, then everything behaves as expected.  This suggests to me that Clap is accepting any member of the command group, instead of the one specifically named in the `requires` annotation.

### Debug Output

```console
➜  clap-test git:(master) ✗ cargo run -- --write --show-hex
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/clap-test --write --show-hex`
[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="clap-test"
[      clap::builder::command] 	Command::_propagate:clap-test
[      clap::builder::command] 	Command::_check_help_and_version:clap-test expand_help_tree=false
[      clap::builder::command] 	Command::long_help_exists
[      clap::builder::command] 	Command::_check_help_and_version: Building default --help
[      clap::builder::command] 	Command::_propagate_global_args:clap-test
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:read
[clap::builder::debug_asserts] 	Arg::_debug_asserts:write
[clap::builder::debug_asserts] 	Arg::_debug_asserts:show_hex
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--write")' ([45, 45, 119, 114, 105, 116, 101])
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("--write")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::parse_long_arg
[        clap::parser::parser] 	Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser] 	Parser::parse_long_arg: Found valid arg or flag '--write'
[        clap::parser::parser] 	Parser::parse_long_arg("write"): Presence validated
[        clap::parser::parser] 	Parser::react action=SetTrue, identifier=Some(Long), source=CommandLine
[        clap::parser::parser] 	Parser::react: has default_missing_vals
[        clap::parser::parser] 	Parser::remove_overrides: id="write"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="write", source=CommandLine
[      clap::builder::command] 	Command::groups_for_arg: id="write"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="Args", source=CommandLine
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="command", source=CommandLine
[        clap::parser::parser] 	Parser::push_arg_values: ["true"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[        clap::parser::parser] 	Parser::get_matches_with: After parse_long_arg ValuesDone
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--show-hex")' ([45, 45, 115, 104, 111, 119, 45, 104, 101, 120])
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("--show-hex")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::parse_long_arg
[        clap::parser::parser] 	Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser] 	Parser::parse_long_arg: Found valid arg or flag '--show-hex'
[        clap::parser::parser] 	Parser::parse_long_arg("show-hex"): Presence validated
[        clap::parser::parser] 	Parser::react action=SetTrue, identifier=Some(Long), source=CommandLine
[        clap::parser::parser] 	Parser::react: has default_missing_vals
[        clap::parser::parser] 	Parser::remove_overrides: id="show_hex"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="show_hex", source=CommandLine
[      clap::builder::command] 	Command::groups_for_arg: id="show_hex"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="Args", source=CommandLine
[        clap::parser::parser] 	Parser::push_arg_values: ["true"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=2
[        clap::parser::parser] 	Parser::get_matches_with: After parse_long_arg ValuesDone
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:read:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:read: has default vals
[        clap::parser::parser] 	Parser::add_default_value:iter:read: wasn't used
[        clap::parser::parser] 	Parser::react action=SetTrue, identifier=None, source=DefaultValue
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="read", source=DefaultValue
[        clap::parser::parser] 	Parser::push_arg_values: ["false"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=3
[        clap::parser::parser] 	Parser::add_defaults:iter:write:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:write: has default vals
[        clap::parser::parser] 	Parser::add_default_value:iter:write: was used
[        clap::parser::parser] 	Parser::add_defaults:iter:show_hex:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:show_hex: has default vals
[        clap::parser::parser] 	Parser::add_default_value:iter:show_hex: was used
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[     clap::parser::validator] 	Validator::validate
[      clap::builder::command] 	Command::groups_for_arg: id="write"
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id="write", conflicts=["read"]
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id="Args", conflicts=[]
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id="command", conflicts=[]
[      clap::builder::command] 	Command::groups_for_arg: id="show_hex"
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id="show_hex", conflicts=[]
[     clap::parser::validator] 	Validator::validate_conflicts
[     clap::parser::validator] 	Validator::validate_exclusive
[     clap::parser::validator] 	Validator::validate_exclusive:iter:"write"
[     clap::parser::validator] 	Validator::validate_exclusive:iter:"Args"
[     clap::parser::validator] 	Validator::validate_exclusive:iter:"command"
[     clap::parser::validator] 	Validator::validate_exclusive:iter:"show_hex"
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id="write"
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg="write"
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id="show_hex"
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg="show_hex"
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator] 	Validator::gather_requires
[     clap::parser::validator] 	Validator::gather_requires:iter:"write"
[     clap::parser::validator] 	Validator::gather_requires:iter:"Args"
[     clap::parser::validator] 	Validator::gather_requires:iter:"Args":group
[     clap::parser::validator] 	Validator::gather_requires:iter:"command"
[     clap::parser::validator] 	Validator::gather_requires:iter:"command":group
[     clap::parser::validator] 	Validator::gather_requires:iter:"show_hex"
[     clap::parser::validator] 	Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator] 	Validator::validate_required:iter:aog="read"
[     clap::parser::validator] 	Validator::validate_required:iter: This is an arg
[     clap::parser::validator] 	Validator::is_missing_required_ok: read
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg="read"
[      clap::builder::command] 	Command::groups_for_arg: id="read"
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id="read", conflicts=["write"]
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=["write", "write"]
[     clap::parser::validator] 	Validator::is_missing_required_ok: true (self)
[   clap::parser::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[]
Args { read: false, write: true, show_hex: true }
```

---

_Label `C-bug` added by @mkeeter on 2023-02-13 16:49_

---

_Comment by @epage on 2023-02-13 17:07_

I think this is a duplicate of #4520.  If not, let us know in what way it is different and we can re-open

---

_Closed by @epage on 2023-02-13 17:07_

---

_Comment by @mkeeter on 2023-02-13 18:19_

Hmmm, I had found that issue when searching, but I'm not sure if it's the same thing.

#4520 is triggered when you use `requires` and `conflicts_with` together; this issue is when you use `requires` and `group` together.

It may be that it's the same issue under the hood, but I don't know enough about Clap internals to say for sure.

---

_Reopened by @epage on 2023-02-13 18:58_

---

_Comment by @epage on 2023-02-13 18:58_

They are both forms of conflicts but I've re-opened this for the sake of making sure we have tests for both use cases.

---

_Label `A-parsing` added by @epage on 2023-02-13 18:58_

---

_Label `A-parsing` removed by @epage on 2023-02-13 18:58_

---

_Label `A-validators` added by @epage on 2023-02-13 18:58_

---

_Comment by @TD-Sky on 2023-07-21 15:18_

Same problem.

---

_Renamed from "`requires` will accept any item from a `group`" to "`requires` is bypassed when other items from a mutually-exclusive `group` are present" by @epage on 2023-07-21 15:33_

---

_Referenced in [clap-rs/clap#5041](../../clap-rs/clap/issues/5041.md) on 2023-07-22 16:00_

---

_Comment by @Xophmeister on 2023-08-17 13:05_

I had the same problem, but I was able to sort-of solve it by lifting the branch that admits options into its own struct. Something like this:

```rust
use clap::{ArgGroup, Args, Parser};

#[derive(Args, Debug)]
struct Foo {
    #[arg(long)]
    foo: String,

    #[arg(long, requires = "foo")]
    quux: Option<String>,
}

#[derive(Debug, Parser)]
#[command(group = ArgGroup::new("mx")
    .multiple(false)
    .required(true)
    .args(&["foo", "bar"])
)]
struct Cli {
    #[command(flatten)]
    foo_w_option: Option<Foo>,

    #[arg(long)]
    bar: Option<String>,
}

fn main() {
    let args = Cli::parse();
    println!("{args:?}");
}
```

I say "sort-of" solve it because, while the parser correctly forbids the `--bar X --quux Y` case, the error message is not very clear:

```
error: The following required argument was not provided: foo

Usage: clap-mx-group-w-option [OPTIONS] <--foo <FOO>|--bar <BAR>>

For more information, try '--help'.
```

This is actually OK -- although I would expect to see `<--foo <FOO> [--quux <QUUX>]|--bar <BAR>>` -- but in my actual use-case, I'm using subcommands and including positional arguments in the mutual exclusivity group. This reverts the usage text to the top-level, rather than the subcommand's usage text. (I don't know if that's worth an issue in its own right...)

---

_Referenced in [clap-rs/clap#5910](../../clap-rs/clap/issues/5910.md) on 2025-02-17 18:58_

---

_Referenced in [clap-rs/clap#6050](../../clap-rs/clap/pulls/6050.md) on 2025-06-27 12:54_

---

_Label `S-blocked` added by @epage on 2025-07-03 16:07_

---

_Comment by @epage on 2025-07-03 16:07_

To make the situation more clear, I've marked this issue as blocked as we need to resolve the analysis in #4520 first.

---

_Referenced in [coconut-svsm/svsm#879](../../coconut-svsm/svsm/pulls/879.md) on 2025-11-20 15:56_

---
