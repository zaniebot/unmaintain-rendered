---
number: 4671
title: "`disable_colored_help` doesn't work"
type: issue
state: closed
author: danteissaias
labels:
  - C-bug
assignees: []
created_at: 2023-01-24T21:41:04Z
updated_at: 2023-01-24T22:22:14Z
url: https://github.com/clap-rs/clap/issues/4671
synced_at: 2026-01-10T01:27:59Z
---

# `disable_colored_help` doesn't work

---

_Issue opened by @danteissaias on 2023-01-24 21:41_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.65.0 (897e37553 2022-11-02)

### Clap Version

4.1.3

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Parser, Debug)]
#[command(
    version,
    disable_help_subcommand = true,
    disable_colored_help = true,
)]
struct Cli {
    #[clap(subcommand)]
    command: Command,
}

#[derive(Subcommand, Debug)]
enum Command {
    Example { arg: String },
}

fn main() {
    let args = Cli::parse();
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

Shows coloured output (bold/underline)

### Expected Behaviour

Should be unstyled 

### Additional Context

`cargo run -- --help` works as expected

### Debug Output

```
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="macpkg"
[      clap::builder::command]  Command::_propagate:macpkg
[      clap::builder::command]  Command::_check_help_and_version:macpkg expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_check_help_and_version: Building default --version
[      clap::builder::command]  Command::_propagate_global_args:macpkg
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Arg::_debug_asserts:version
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::add_defaults
[        clap::parser::parser]  Parser::add_defaults:iter:help:
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:version:
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:version: doesn't have default vals
[     clap::parser::validator]  Validator::validate
[      clap::builder::command]  Command::write_help_err: macpkg, use_long=false
[          clap::output::help]  write_help
[ clap::output::help_template]  HelpTemplate::new cmd=macpkg, use_long=false
[ clap::output::help_template]  should_show_arg: use_long=false, arg=help
[ clap::output::help_template]  HelpTemplate::write_templated_help
[ clap::output::help_template]  HelpTemplate::write_before_help
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::needs_options_tag
[         clap::output::usage]  Usage::needs_options_tag:iter: f=help
[         clap::output::usage]  Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage]  Usage::needs_options_tag:iter: f=version
[         clap::output::usage]  Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage]  Usage::needs_options_tag: [OPTIONS] not required
[         clap::output::usage]  Usage::get_args: incls=[]
[         clap::output::usage]  Usage::get_args: unrolled_reqs=[]
[         clap::output::usage]  Usage::get_args: ret_val=[]
[         clap::output::usage]  Usage::create_help_usage: usage=macpkg <COMMAND>
[ clap::output::help_template]  HelpTemplate::write_all_args
[ clap::output::help_template]  should_show_arg: use_long=false, arg=help
[ clap::output::help_template]  should_show_arg: use_long=false, arg=version
[ clap::output::help_template]  HelpTemplate::write_subcommands
[ clap::output::help_template]  HelpTemplate::write_subcommands longest = 7
[ clap::output::help_template]  HelpTemplate::sc_spec_vals: a=build
[ clap::output::help_template]  HelpTemplate::sc_spec_vals: a=install
[ clap::output::help_template]  HelpTemplate::write_subcommand
[ clap::output::help_template]  HelpTemplate::sc_spec_vals: a=build
[ clap::output::help_template]  HelpTemplate::help
[ clap::output::help_template]  HelpTemplate::help: help_width=15, spaces=15, avail=85
[ clap::output::help_template]  HelpTemplate::write_subcommand
[ clap::output::help_template]  HelpTemplate::sc_spec_vals: a=install
[ clap::output::help_template]  HelpTemplate::help
[ clap::output::help_template]  HelpTemplate::help: help_width=15, spaces=17, avail=85
[ clap::output::help_template]  HelpTemplate::write_args Options
[ clap::output::help_template]  should_show_arg: use_long=false, arg=help
[ clap::output::help_template]  HelpTemplate::write_args: arg="help" longest=6
[ clap::output::help_template]  should_show_arg: use_long=false, arg=version
[ clap::output::help_template]  HelpTemplate::write_args: arg="version" longest=9
[ clap::output::help_template]  should_show_arg: use_long=false, arg=help
[ clap::output::help_template]  HelpTemplate::spec_vals: a=--help
[ clap::output::help_template]  should_show_arg: use_long=false, arg=version
[ clap::output::help_template]  HelpTemplate::spec_vals: a=--version
[ clap::output::help_template]  HelpTemplate::spec_vals: a=--help
[ clap::output::help_template]  HelpTemplate::short
[ clap::output::help_template]  HelpTemplate::long
[ clap::output::help_template]  HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=9
[ clap::output::help_template]  HelpTemplate::align_to_about: positional=false arg_len=6, spaces=5
[ clap::output::help_template]  HelpTemplate::help
[ clap::output::help_template]  HelpTemplate::help: help_width=17, spaces=10, avail=83
[ clap::output::help_template]  HelpTemplate::spec_vals: a=--version
[ clap::output::help_template]  HelpTemplate::short
[ clap::output::help_template]  HelpTemplate::long
[ clap::output::help_template]  HelpTemplate::align_to_about: arg=version, next_line_help=false, longest=9
[ clap::output::help_template]  HelpTemplate::align_to_about: positional=false arg_len=9, spaces=2
[ clap::output::help_template]  HelpTemplate::help
[ clap::output::help_template]  HelpTemplate::help: help_width=17, spaces=13, avail=83
[ clap::output::help_template]  HelpTemplate::write_after_help
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
  ```

---

_Label `C-bug` added by @danteissaias on 2023-01-24 21:41_

---

_Renamed from "`disable_help_subcommand` doesn't work" to "`disable_colored_help` doesn't work" by @danteissaias on 2023-01-24 21:44_

---

_Referenced in [clap-rs/clap#4672](../../clap-rs/clap/pulls/4672.md) on 2023-01-24 21:53_

---

_Closed by @epage on 2023-01-24 22:22_

---

_Referenced in [clap-rs/clap#4674](../../clap-rs/clap/issues/4674.md) on 2023-01-25 00:13_

---
