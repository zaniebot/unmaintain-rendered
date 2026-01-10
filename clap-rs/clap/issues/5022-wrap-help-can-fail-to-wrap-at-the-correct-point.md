---
number: 5022
title: wrap_help can fail to wrap at the correct point
type: issue
state: closed
author: Xophmeister
labels:
  - C-bug
assignees: []
created_at: 2023-07-19T15:07:10Z
updated_at: 2023-07-19T16:04:53Z
url: https://github.com/clap-rs/clap/issues/5022
synced_at: 2026-01-10T01:28:05Z
---

# wrap_help can fail to wrap at the correct point

---

_Issue opened by @Xophmeister on 2023-07-19 15:07_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.70.0 (90c541806 2023-05-31) (built from a source tarball)

### Clap Version

4.3.16

### Minimal reproducible code

```rust
use clap::{Parser, ValueEnum};

#[derive(Debug, Parser)]
struct Cli {
    #[arg(short)]
    foo: Foo,
}

#[derive(Clone, Debug, ValueEnum)]
enum Foo {
    /// Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt
    /// ut labore et dolore magna aliqua. Sed vulputate mi sit amet mauris commodo quis imperdiet
    /// massa.
    Foo,
}

fn main() {
    let cli = Cli::parse();
    println!("{cli:?}");
}
```

### Steps to reproduce the bug with the above code

Set your terminal to 90 columns, for example, then:

```
cargo run -- --help
```

### Actual Behaviour

Output (manually hard-wrapped for sake of demonstration):
```
Usage: example -f <FOO>

Options:
  -f <FOO>
          Possible values:
          - foo: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod t
empor
            incididunt ut labore et dolore magna aliqua. Sed vulputate mi sit amet mauris
            commodo quis imperdiet massa

  -h, --help
          Print help (see a summary with '-h')
```

### Expected Behaviour

Output:
```
Usage: example -f <FOO>

Options:
  -f <FOO>
          Possible values:
          - foo: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
            tempor incididunt ut labore et dolore magna aliqua. Sed vulputate mi sit amet
            mauris commodo quis imperdiet massa

  -h, --help
          Print help (see a summary with '-h')
```

### Additional Context

Tested in Zsh 5.8.1 and Bash 5.1.16, both running inside and outside Tmux 3.2a.

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="example"
[clap_builder::builder::command]Command::_propagate:example
[clap_builder::builder::command]Command::_check_help_and_version:example expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:example
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:foo
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
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
[clap_builder::builder::command]Command::long_help_exists: true
[clap_builder::builder::command]Command::write_help_err: example, use_long=true
[clap_builder::builder::command]Command::long_help_exists: true
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=example, use_long=true
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=foo
[clap_builder::output::help_template]HelpTemplate::write_templated_help
[clap_builder::output::help_template]HelpTemplate::write_before_help
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_help_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=foo
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is required
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=help
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::get_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["foo"]
[ clap_builder::output::usage]Usage::get_args: ret_val=[StyledStr("\u{1b}[1m-f\u{1b}[0m <FOO>")]
[ clap_builder::output::usage]Usage::create_help_usage: usage=example -f <FOO>
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=foo
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args Options
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=foo
[clap_builder::output::help_template]HelpTemplate::write_args: arg="foo" longest=8
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args: arg="help" longest=8
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=foo
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=-f <FOO>
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=-f <FOO>
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=foo, next_line_help=true, longest=8
[clap_builder::output::help_template]HelpTemplate::align_to_about: printing long help so skip alignment
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: Next Line...true
[clap_builder::output::help_template]HelpTemplate::help: help_width=10, spaces=0, avail=80
[clap_builder::output::help_template]HelpTemplate::help: Found possible vals...[PossibleValue { name: "foo", help: Some(StyledStr("Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Sed vulputate mi sit amet mauris commodo quis imperdiet massa")), aliases: [], hide: false }]
[clap_builder::output::help_template]HelpTemplate::help: Possible Value help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=true, longest=8
[clap_builder::output::help_template]HelpTemplate::align_to_about: printing long help so skip alignment
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: Next Line...true
[clap_builder::output::help_template]HelpTemplate::help: help_width=10, spaces=36, avail=80
[clap_builder::output::help_template]HelpTemplate::write_after_help
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
```

---

_Label `C-bug` added by @Xophmeister on 2023-07-19 15:07_

---

_Comment by @epage on 2023-07-19 15:11_

For a full reproduction case:
`````rust
#!/usr/bin/env nargo
/*!
```cargo
[dependencies]
clap = { version = "4", features = ["derive", "wrap_help"] }
```
*/

use clap::{Parser, ValueEnum};

#[derive(Debug, Parser)]
struct Cli {
    #[arg(short)]
    foo: Foo,
}

#[derive(Clone, Debug, ValueEnum)]
enum Foo {
    /// Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt
    /// ut labore et dolore magna aliqua. Sed vulputate mi sit amet mauris commodo quis imperdiet
    /// massa.
    Foo,
}

fn main() {
    let cli = Cli::parse();
    println!("{cli:?}");
}
`````
(`nargo` is just a shell script around `cargo` that enables the nightly feature for this)

---

_Referenced in [clap-rs/clap#5023](../../clap-rs/clap/pulls/5023.md) on 2023-07-19 15:42_

---

_Closed by @epage on 2023-07-19 16:04_

---
