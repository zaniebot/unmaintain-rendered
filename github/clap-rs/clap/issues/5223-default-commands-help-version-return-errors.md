---
number: 5223
title: Default commands (help, version) return errors
type: issue
state: closed
author: hugoclrd
labels:
  - C-bug
assignees: []
created_at: 2023-11-24T08:55:55Z
updated_at: 2025-09-26T20:31:28Z
url: https://github.com/clap-rs/clap/issues/5223
synced_at: 2026-01-07T13:12:20-06:00
---

# Default commands (help, version) return errors

---

_Issue opened by @hugoclrd on 2023-11-24 08:55_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.74.0 (79e9716c9 2023-11-13)

### Clap Version

4.4.4

### Minimal reproducible code

```rust
use clap::{CommandFactory, Parser, Subcommand};

#[derive(Subcommand)]
enum Command {
    /// Create and scaffold a new project
    #[command(name = "new", bin_name = "new")]
    New { name: String },
}

#[derive(Parser)]
#[command(version, name = "proj", bin_name = "proj")]
struct Opts {
    #[command(subcommand)]
    command: Command,
}

fn main() {
    let mut cmd = Opts::command();

    let r = cmd.try_get_matches_from_mut(["proj", "new", "idea"]);
    println!("r: {}", r.is_ok()); // true
    let r = cmd.try_get_matches_from_mut(["proj", "help"]);
    println!("r: {}", r.is_ok()); // false
    let r = cmd.try_get_matches_from_mut(["proj", "version"]);
    println!("r: {}", r.is_ok()); // false
    let r = cmd.try_get_matches_from_mut(["proj", "new", "--help"]);
    println!("r: {}", r.is_ok()); // false
    println!("r: {:?}", r);
    // it does print the actual help but as an error
    // Err(ErrorInner { kind: DisplayHelp, ..., message: Some(Formatted(StyledStr("Create and scaffold a new project\n\ ...
}

```


### Steps to reproduce the bug with the above code

```
cargo run
```

### Actual Behaviour

The default command (`version` and `help`) returns the right context but as Err. The message are therefore displayed as failures in my terminal (in red)

### Expected Behaviour

Should return `Ok` 

### Additional Context

_No response_

### Debug Output

```console
   Compiling clap_complete v4.4.4
   Compiling hello_world v0.1.0 (.../Sites/hiro/hello_world)
    Finished dev [unoptimized + debuginfo] target(s) in 0.80s
     Running `target/debug/hello_world`
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="proj"
[clap_builder::builder::command]Command::_propagate:proj
[clap_builder::builder::command]Command::_check_help_and_version:proj expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:proj
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:version
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"new"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("new")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("new")
[clap_builder::parser::parser]Parser::parse_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of new to "proj new"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of new to "proj-new"
[clap_builder::builder::command]Command::_build: name="new"
[clap_builder::builder::command]Command::_propagate:new
[clap_builder::builder::command]Command::_check_help_and_version:new expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:new
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:name
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=new
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"idea"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("idea")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]Parser::resolve_pending: id="name"
[clap_builder::parser::parser]Parser::react action=Set, identifier=Some(Index), source=CommandLine
[clap_builder::parser::parser]Parser::remove_overrides: id="name"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="name", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="name"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="New", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["idea"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=name, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:name:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:name: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="name"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="name", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="New", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="name"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="name"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([Child { id: "name", children: [] }])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"name"
[clap_builder::parser::validator]Validator::gather_requires:iter:"New"
[clap_builder::parser::validator]Validator::gather_requires:iter:"New":group
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:version:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:version: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::arg_matcher]ArgMatcher::get_global_values: global_arg_vec=[]
r: true
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="proj"
[clap_builder::builder::command]Command::_build: already built
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"help"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("help")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("help")
[clap_builder::parser::parser]Parser::parse_help_subcommand
[clap_builder::builder::command]Command::long_help_exists: false
[clap_builder::builder::command]Command::write_help_err: proj, use_long=false
[clap_builder::builder::command]Command::long_help_exists: false
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=proj, use_long=false
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_templated_help
[clap_builder::output::help_template]HelpTemplate::write_before_help
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=help
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=version
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_no_title: usage=proj <COMMAND>
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=version
[clap_builder::output::help_template]HelpTemplate::write_subcommands
[clap_builder::output::help_template]HelpTemplate::write_subcommands longest = 4
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=new
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=help
[clap_builder::output::help_template]HelpTemplate::write_subcommand
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=new
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=12, spaces=33, avail=88
[clap_builder::output::help_template]HelpTemplate::write_subcommand
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=help
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=12, spaces=57, avail=88
[clap_builder::output::help_template]HelpTemplate::write_args Options
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args: arg="help" longest=6
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=version
[clap_builder::output::help_template]HelpTemplate::write_args: arg="version" longest=9
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=version
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--version
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=9
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=6, spaces=5
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=17, spaces=10, avail=83
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--version
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=version, next_line_help=false, longest=9
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=9, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=17, spaces=13, avail=83
[clap_builder::output::help_template]HelpTemplate::write_after_help
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
r: false
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="proj"
[clap_builder::builder::command]Command::_build: already built
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"version"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("version")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=help
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=version
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: proj <COMMAND>
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
r: false
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="proj"
[clap_builder::builder::command]Command::_build: already built
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"new"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("new")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("new")
[clap_builder::parser::parser]Parser::parse_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of new to "proj new"
[clap_builder::builder::command]Command::_build: name="new"
[clap_builder::builder::command]Command::_build: already built
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=new
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--help"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--help")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--help'
[clap_builder::parser::parser]Parser::parse_long_arg("help"): Presence validated
[clap_builder::parser::parser]Parser::react action=Help, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Help: use_long=true
[clap_builder::builder::command]Command::long_help_exists: false
[clap_builder::builder::command]Command::write_help_err: proj-new, use_long=false
[clap_builder::builder::command]Command::long_help_exists: false
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=new, use_long=false
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=name
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_templated_help
[clap_builder::output::help_template]HelpTemplate::write_before_help
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=help
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["name"]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_no_title: usage=proj new <NAME>
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=name
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args Arguments
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=name
[clap_builder::output::help_template]HelpTemplate::write_args: arg="name" longest=6
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=name
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=<NAME>
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=<NAME>
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=name, next_line_help=false, longest=6
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=true arg_len=6, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=10, spaces=0, avail=90
[clap_builder::output::help_template]HelpTemplate::write_args Options
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args: arg="help" longest=6
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=6
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=6, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=14, spaces=10, avail=86
[clap_builder::output::help_template]HelpTemplate::write_after_help
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
r: false
r: Err(ErrorInner { kind: DisplayHelp, context: FlatMap { keys: [], values: [] }, message: Some(Formatted(StyledStr("Create and scaffold a new project\n\n\u{1b}[1m\u{1b}[4mUsage:\u{1b}[0m \u{1b}[1mproj new\u{1b}[0m <NAME>\n\n\u{1b}[1m\u{1b}[4mArguments:\u{1b}[0m\n  <NAME>  \n\n\u{1b}[1m\u{1b}[4mOptions:\u{1b}[0m\n  \u{1b}[1m-h\u{1b}[0m, \u{1b}[1m--help\u{1b}[0m  Print help\n"))), source: None, help_flag: Some("--help"), styles: Styles { header: Style { fg: None, bg: None, underline: None, effects: Effects(BOLD | UNDERLINE) }, error: Style { fg: Some(Ansi(Red)), bg: None, underline: None, effects: Effects(BOLD) }, usage: Style { fg: None, bg: None, underline: None, effects: Effects(BOLD | UNDERLINE) }, literal: Style { fg: None, bg: None, underline: None, effects: Effects(BOLD) }, placeholder: Style { fg: None, bg: None, underline: None, effects: Effects() }, valid: Style { fg: Some(Ansi(Green)), bg: None, underline: None, effects: Effects() }, invalid: Style { fg: Some(Ansi(Yellow)), bg: None, underline: None, effects: Effects() } }, color_when: Auto, color_help_when: Auto, backtrace: Some(Backtrace(   0: backtrace::backtrace::libunwind::trace
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/backtrace-0.3.69/src/backtrace/libunwind.rs:93:5
      backtrace::backtrace::trace_unsynchronized
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/backtrace-0.3.69/src/backtrace/mod.rs:66:5
   1: backtrace::backtrace::trace
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/backtrace-0.3.69/src/backtrace/mod.rs:53:14
   2: backtrace::capture::Backtrace::create
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/backtrace-0.3.69/src/capture.rs:176:9
   3: backtrace::capture::Backtrace::new
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/backtrace-0.3.69/src/capture.rs:140:22
   4: clap_builder::error::Backtrace::new
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.8/src/error/mod.rs:890:19
   5: clap_builder::error::Error<F>::new
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.8/src/error/mod.rs:140:28
   6: clap_builder::error::Error<F>::for_app
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.8/src/error/mod.rs:294:9
   7: clap_builder::error::Error<F>::display_help
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.8/src/error/mod.rs:351:9
   8: clap_builder::parser::parser::Parser::help_err
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.8/src/parser/parser.rs:1563:9
   9: clap_builder::parser::parser::Parser::react
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.8/src/parser/parser.rs:1256:21
  10: clap_builder::parser::parser::Parser::parse_long_arg
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.8/src/parser/parser.rs:801:17
  11: clap_builder::parser::parser::Parser::get_matches_with
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.8/src/parser/parser.rs:115:44
  12: clap_builder::parser::parser::Parser::parse_subcommand
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.8/src/parser/parser.rs:693:37
  13: clap_builder::parser::parser::Parser::get_matches_with
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.8/src/parser/parser.rs:458:17
  14: clap_builder::builder::command::Command::_do_parse
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.8/src/builder/command.rs:3961:29
  15: clap_builder::builder::command::Command::try_get_matches_from_mut
             at .../.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.8/src/builder/command.rs:791:9
  16: hello_world::main
             at src/main.rs:26:13
  17: core::ops::function::FnOnce::call_once
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/ops/function.rs:250:5
  18: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/sys_common/backtrace.rs:154:18
  19: std::rt::lang_start::{{closure}}
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/rt.rs:166:18
  20: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/ops/function.rs:284:13
      std::panicking::try::do_call
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:504:40
      std::panicking::try
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:468:19
      std::panic::catch_unwind
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panic.rs:142:14
      std::rt::lang_start_internal::{{closure}}
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/rt.rs:148:48
      std::panicking::try::do_call
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:504:40
      std::panicking::try
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:468:19
      std::panic::catch_unwind
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panic.rs:142:14
      std::rt::lang_start_internal
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/rt.rs:148:20
  21: std::rt::lang_start
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/rt.rs:165:17
  22: _main
)) })
```

---

_Label `C-bug` added by @hugoclrd on 2023-11-24 08:55_

---

_Renamed from "Default commands (help, version) " to "Default commands (help, version) return errors" by @hugoclrd on 2023-11-27 15:46_

---

_Comment by @epage on 2023-11-27 19:33_

> The default command (version and help) returns the right context but as Err. The message are therefore displayed as failures in my terminal (in red)

Steps are missing to reproduce this but I'm going to make some guesses.

I'm assuming you are using the `try_` variants, getting the `Result`, and setting the exist code based on `r.is_ok()` and your terminal then marks it as red.

It is expected behavior that `help`, `--help`, `-h`, `--version`, and `-V` are `Result::Err`s. This ensures that they correctly bubble up through the stack (and even the program) and are mutually exclusive with `Result::Ok` which is `ArgMatches`.

If you look at [Error::exit](https://github.com/clap-rs/clap/blob/master/clap_builder/src/error/mod.rs#L233), you'll see how we deal with this for setting the appropriate exit code which you can do in your own program.

As this is expected behavior, I'm closing.

---

_Closed by @epage on 2023-11-27 19:33_

---

_Comment by @hugoclrd on 2023-11-27 21:53_

Ok so it is "expected behavior" that `try_` returns `Err`s here but not that the terminal show it as such, if I understand correctly

If so, I made a wrong assumption, it doesn't mean that there is no issue 🤔

I'll open a new issue once I figure out what's wrong then

---

_Comment by @epage on 2023-11-27 21:59_

If you are using `try_` functions, what is going wrong between the `Result` and your terminal is up to you, clap has no say in that matter.

---

_Comment by @hugoclrd on 2023-11-27 22:13_

Okay thanks, I must have mis-understood what `try_` does. I'll open an other issue if I can't figure my bug out

---

_Comment by @hugoclrd on 2023-11-27 22:14_

Thanks for linking the Error::exit method, that's what I needed

---
