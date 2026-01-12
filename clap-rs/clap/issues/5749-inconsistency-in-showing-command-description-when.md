```yaml
number: 5749
title: "Inconsistency in showing command description when using `about`"
type: issue
state: open
author: teamplayer3
labels:
  - C-bug
  - S-waiting-on-decision
  - A-derive
assignees: []
created_at: 2024-09-25T14:03:31Z
updated_at: 2024-09-30T08:22:58Z
url: https://github.com/clap-rs/clap/issues/5749
synced_at: 2026-01-12T16:14:17Z
```

# Inconsistency in showing command description when using `about`

---

_@teamplayer3_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.82.0-nightly (f167efad2 2024-08-24)

### Clap Version

4.5.18

### Minimal reproducible code

```toml
[package]
name = "clap-bug"
version = "0.1.0"
edition = "2021"

description = """
This is from about section.
"""

[dependencies]
clap = { version = "4.5.18", features = ["derive"] }
```

```rust
use clap::{Args, Parser};

#[derive(Parser, Debug)]
#[command(version, about)]
pub struct Cli {
    #[command(subcommand)]
    pub command: CargoInvocation,
}

#[derive(Parser, Debug)]
pub enum CargoInvocation {
    #[command(name = "a-cmd", about)]
    A(A),
    /// This is a test.
    #[command(name = "b-cmd")]
    B(B),
}

#[derive(Args, Debug)]
pub struct A {}

#[derive(Args, Debug)]
pub struct B {}

fn main() {
    let args = Cli::parse();
}

```


### Steps to reproduce the bug with the above code

```bash
cargo run -- --help
```

### Actual Behaviour

```
This is from about section.


Usage: clap-bug.exe <COMMAND>

Commands:
  a-cmd  This is from about section.

  b-cmd  This is a test
  help   Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```

### Expected Behaviour

The `.` at the command description of `a-cmd` should get removed, as with the other descriptions.

```
This is from about section.


Usage: clap-bug.exe <COMMAND>

Commands:
  a-cmd  This is from about section

  b-cmd  This is a test
  help   Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```

### Additional Context

_No response_

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
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
[clap_builder::builder::command]Command::write_help_err: clap-bug, use_long=false
[clap_builder::builder::command]Command::long_help_exists: false
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=clap-bug, use_long=false
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
[ clap_builder::output::usage]Usage::create_usage_no_title: usage=clap-bug.exe <COMMAND>
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=version
[clap_builder::output::help_template]HelpTemplate::write_subcommands
[clap_builder::output::help_template]HelpTemplate::write_subcommands longest = 5
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=a-cmd
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=b-cmd
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=help
[clap_builder::output::help_template]HelpTemplate::write_subcommand
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=a-cmd
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=9, spaces=27, avail=91
[clap_builder::output::help_template]HelpTemplate::write_subcommand
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=b-cmd
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=9, spaces=14, avail=91
[clap_builder::output::help_template]HelpTemplate::write_subcommand
[clap_builder::output::help_template]HelpTemplate::sc_spec_vals: a=help
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=9, spaces=57, avail=91
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
```

---

_Label `C-bug` added by @teamplayer3 on 2024-09-25 14:03_

---

_Label `A-derive` added by @epage on 2024-09-25 14:26_

---

_Label `S-waiting-on-decision` added by @epage on 2024-09-25 14:26_

---

_Comment by @epage on 2024-09-25 14:33_

Yes, pulling from the env variable takes it literally for the `about`
https://github.com/clap-rs/clap/blob/1c0e602d8060a5feeb82acd6796490ddb3bc1a79/clap_derive/src/item.rs#L517-L521
The builder equivalent also doesn't do anything special
https://github.com/clap-rs/clap/blob/1c0e602d8060a5feeb82acd6796490ddb3bc1a79/clap_builder/src/macros.rs#L60-L78

While we always remove the period when extracting an `about` from doc comments
https://github.com/clap-rs/clap/blob/1c0e602d8060a5feeb82acd6796490ddb3bc1a79/clap_derive/src/utils/doc_comments.rs#L71-L79

doc comments also have us trim later paragraphs.

I'm also curious what common conventions are for each and how changing this might impact them.

---

_Comment by @teamplayer3 on 2024-09-30 08:22_

I think the more common approach is to display these descriptions without a `.`.

---
