---
number: 5984
title: "Multiple `from_global` within a `Command` causes \"required argument\" error with `clap_derive`"
type: issue
state: open
author: soemre
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2025-05-04T22:34:55Z
updated_at: 2025-05-05T19:02:59Z
url: https://github.com/clap-rs/clap/issues/5984
synced_at: 2026-01-10T01:28:20Z
---

# Multiple `from_global` within a `Command` causes "required argument" error with `clap_derive`

---

_Issue opened by @soemre on 2025-05-04 22:34_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.86.0 (05f9846f8 2025-03-31)

### Clap Version

4.5.37

### Minimal reproducible code

# Case 1

```rust
use clap::{Args, Parser, Subcommand, arg, command};

#[derive(Parser)]
struct Cli {
    #[command(flatten)]
    globals: GlobalArgs,

    #[command(subcommand)]
    command: Commands,
}

#[derive(Args)]
struct GlobalArgs {
    #[arg(short, global = true)]
    bar: bool,
}

#[derive(Subcommand)]
enum Commands {
    Foo(#[command(flatten)] FooArgs),
}

#[derive(Args)]
struct FooArgs {
    // Commenting out this part will make it work again
    #[arg(from_global)]
    bar: bool,

    // Or this part
    #[arg(from_global, name = "bar")]
    bar_two: bool,
}

fn main() {
    Cli::parse();
    println!("not failed")
}
```
# Case 2

```rust
use clap::{Args, Parser, Subcommand, arg, command};

#[derive(Parser)]
struct Cli {
    #[command(flatten)]
    globals: GlobalArgs,

    #[command(subcommand)]
    command: Commands,
}

#[derive(Args)]
struct GlobalArgs {
    #[arg(short, global = true)]
    bar: bool,
}

#[derive(Subcommand)]
enum Commands {
    Foo(#[command(flatten)] FooArgs),
}

#[derive(Args)]
struct FooArgs {
    // Commenting out this part will make it work again
    #[arg(from_global)]
    bar: bool,

    // Or this part
    #[command(flatten)]
    sub_proc_args: SubProcArgs,
}

#[derive(Args)]
struct SubProcArgs {
    #[arg(from_global)]
    bar: bool,
}

fn main() {
    Cli::parse();
    println!("not failed")
}
```


### Steps to reproduce the bug with the above code

Try `cargo r -- foo` and `cargo r -- foo -b`

### Actual Behaviour

In **Case 1**, when a struct has multiple fields marked with `#[arg(from_global)]` and those fields refer to the same argument name, the program enters a state where it continuously throws errors.

The same issue occurs in **Case 2**, where one of the `from_global` fields is nested under a parent field marked with `#[command(flatten)]`.

This error happens regardless of whether the argument is provided. In fact, the argument doesn't even need to be supplied at all.

### Expected Behaviour

Failing in **Case 1** makes sense however, I think it _shouldn't have compiled in the first place_.  
But the real issue, in my opinion, is **Case 2**.

First of all, **Case 2** becomes quite _hard to debug_ as the codebase grows.  
Beyond that, I believe it breaks the ergonomic expectations of `#[command(flatten)]`.  
I use `flatten` to isolate specific routine data and build functions that only deal with that.  
There are cases where both the parent and child structs need access to global arguments; for example, when both should respect flags like `verbose` or `fail_after`.

That said, I think this weird error state should definitely be fixed.  
As for `#[command(flatten)]`, maybe the current behavior is intentional but if so, it's not what I'd expect.

### Additional Context

The only solution I’ve found is to avoid using `from_global` in the parent struct when `flatten`ed children contain fields with the same `from_global` arguments. Instead, I access the global arguments through the children's fields.

### Debug Output

**Output of `cargo r -- foo -b`:**

```
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/clap-tst foo -b`
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-tst"
[clap_builder::builder::command]Command::_propagate:clap-tst
[clap_builder::builder::command]Command::_check_help_and_version:clap-tst expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:clap-tst
[clap_builder::builder::command]Command::_propagate pushing "bar" to foo
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:bar
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"foo"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("foo")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("foo")
[clap_builder::parser::parser]Parser::parse_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of foo to "clap-tst foo"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of foo to "clap-tst-foo"
[clap_builder::builder::command]Command::_build: name="foo"
[clap_builder::builder::command]Command::_propagate:foo
[clap_builder::builder::command]Command::_check_help_and_version:foo expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:foo
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:bar
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=foo
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-b"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-b")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "b", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['b']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_arg:iter:b
[clap_builder::parser::parser]Parser::parse_short_arg:iter:b: Found valid opt or flag
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=Some(Short), source=CommandLine
[clap_builder::parser::parser]Parser::react: has default_missing_vals
[clap_builder::parser::parser]Parser::remove_overrides: id="bar"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="bar", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="bar"
[clap_builder::parser::parser]Parser::push_arg_values: ["true"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::get_matches_with: After parse_short_arg ValuesDone
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:bar:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:bar: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:bar: was used
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="bar"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="bar", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="bar"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="bar"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"bar"
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:bar:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:bar: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:bar: wasn't used
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=None, source=DefaultValue
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="bar", source=DefaultValue
[clap_builder::parser::parser]Parser::push_arg_values: ["false"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::arg_matcher]ArgMatcher::get_global_values: global_arg_vec=["bar", "bar"]
[clap_builder::builder::command]Command::_build: name="clap-tst"
[clap_builder::builder::command]Command::_propagate:clap-tst
[clap_builder::builder::command]Command::_check_help_and_version:clap-tst expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:clap-tst
[clap_builder::builder::command]Command::_propagate pushing "bar" to foo
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:bar
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::_build: name="clap-tst"
[clap_builder::builder::command]Command::_build: already built
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=bar
[clap_builder::builder::command]Command::groups_for_arg: id="bar"
[ clap_builder::output::usage]Usage::needs_options_tag:iter:iter: grp_s="GlobalArgs"
[ clap_builder::output::usage]Usage::needs_options_tag:iter: [OPTIONS] required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: clap-tst [OPTIONS] <COMMAND>
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: The following required argument was not provided: bar

Usage: clap-tst [OPTIONS] <COMMAND>

For more information, try '--help'.
```

---

_Label `C-bug` added by @soemre on 2025-05-04 22:34_

---

_Label `A-derive` added by @epage on 2025-05-05 18:50_

---

_Renamed from "`#[command(flatten)]` inevitably breaks `#[arg(from_global)]`" to "Multiple `from_global` withi" by @epage on 2025-05-05 19:00_

---

_Renamed from "Multiple `from_global` withi" to "Multiple `from_global` within a `Command` causes "required argument" error with `clap_derive`" by @epage on 2025-05-05 19:00_

---

_Comment by @epage on 2025-05-05 19:02_

The problem comes from clap removing the values from `ArgMatches`, instead of clone, to remove "unnecessary allocations". See #3742.

We could turn that first case into a compile errors but couldn't do it for the second.

As for doing any kind of detection to decide between remove vs get would be tricky to get the heuristics right.

---
