---
number: 5071
title: Author and version not displayed in multicall binary
type: issue
state: closed
author: trou
labels:
  - C-bug
assignees: []
created_at: 2023-08-15T10:25:43Z
updated_at: 2023-08-15T19:11:25Z
url: https://github.com/clap-rs/clap/issues/5071
synced_at: 2026-01-10T01:28:06Z
---

# Author and version not displayed in multicall binary

---

_Issue opened by @trou on 2023-08-15 10:25_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.71.1 (eb26296b5 2023-08-03)

### Clap Version

4.3.21

### Minimal reproducible code

```rust
use clap::{Command};
fn main() {
    let mut app = clap::Command::new("clap-bug")
        .multicall(true)
        .version(env!("CARGO_PKG_VERSION"))
        .propagate_version(true)
        .subcommand(
            Command::new("clap-bug")
                .author("Author Here")
                .about("Program name")
                .arg_required_else_help(true)
                .subcommands([Command::new("list").about("list applets")])
                .subcommand_value_name("APPLET")
                .subcommand_help_heading("APPLETS")
        );

    // Parse args
    app.get_matches_mut();
}
```


### Steps to reproduce the bug with the above code

`cargo run -- -h`

### Actual Behaviour

author and program version are missing in the output:
```
Program name

Usage: clap-bug [APPLET]

APPLETS:
  list  list applets
  help  Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```

### Expected Behaviour

This is the behaviour with clap 3.2.25:
```
clap-bug 0.1.0
Author Here
Program name

USAGE:
    clap-bug [APPLET]

OPTIONS:
    -h, --help       Print help information
    -V, --version    Print version information

APPLETS:
    help    Print this message or the help of the given subcommand(s)
    list    list applets
```

### Additional Context

I also tried to put the author and program name in the main `Command`, but this does not change anything.

### Debug Output

```
[clap_builder::builder::command]Command::try_get_matches_from_mut: Parsed command clap-bug from argv
[clap_builder::builder::command]Command::try_get_matches_from_mut: Reinserting command into arguments so subcommand parser matches it
[clap_builder::builder::command]Command::try_get_matches_from_mut: Clearing name and bin_name so that displayed command name starts with applet name
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name=""
[clap_builder::builder::command]Command::_propagate:
[clap_builder::builder::command]Command::_check_help_and_version: expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"clap-bug"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("clap-bug")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("clap-bug")
[clap_builder::parser::parser]Parser::parse_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of clap-bug to "clap-bug"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of clap-bug to "clap-bug"
[clap_builder::builder::command]Command::_build: name="clap-bug"
[clap_builder::builder::command]Command::_propagate:clap-bug
[clap_builder::builder::command]Command::_check_help_and_version:clap-bug expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:clap-bug
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:version
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=clap-bug
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-h"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-h")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "h", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['h']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_arg:iter:h
[clap_builder::parser::parser]Parser::parse_short_arg:iter:h: Found valid opt or flag
[clap_builder::parser::parser]Parser::react action=Help, identifier=Some(Short), source=CommandLine
[clap_builder::parser::parser]Help: use_long=false
[clap_builder::builder::command]Command::write_help_err: clap-bug, use_long=false
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=clap-bug, use_long=false
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_templated_help
[clap_builder::output::help_template]HelpTemplate::write_before_help
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_help_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=help
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=version
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::get_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_args: ret_val=[]
[ clap_builder::output::usage]Usage::create_help_usage: usage=clap-bug [APPLET]
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=version
[clap_builder::output::help_template]HelpTemplate::write_subcommands
[clap_builder::output::help_template]HelpTemplate::write_subcommands longest = 4
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=list
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=help
[clap_builder::output::help_template]HelpTemplate::write_subcommand
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=list
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=12, spaces=12, avail=88
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
Program name

Usage: clap-bug [APPLET]

APPLETS:
  list  list applets
  help  Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```

---

_Label `C-bug` added by @trou on 2023-08-15 10:25_

---

_Comment by @epage on 2023-08-15 13:16_

This is expected behavior for all programs.  This was changed in [4.0.0](https://github.com/clap-rs/clap/blob/master/CHANGELOG.md#400---2022-09-28) (see also #4132).

See [help_template](https://docs.rs/clap/latest/clap/struct.Command.html#method.help_template) for restoring the old behavior

---

_Closed by @epage on 2023-08-15 13:16_

---

_Comment by @trou on 2023-08-15 19:11_

Oops, sorry. Thanks for the quick reply.

---
