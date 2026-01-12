```yaml
number: 5834
title: "[Bug]: Clap grabs the description from the sub crate in the workspace instead of the main crate the sub crate is being compiled into"
type: issue
state: closed
author: lexi-the-cute
labels:
  - C-bug
assignees: []
created_at: 2024-12-02T09:18:04Z
updated_at: 2024-12-02T16:00:54Z
url: https://github.com/clap-rs/clap/issues/5834
synced_at: 2026-01-12T16:14:17Z
```

# [Bug]: Clap grabs the description from the sub crate in the workspace instead of the main crate the sub crate is being compiled into

---

_@lexi-the-cute_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.83.0 (90b35a623 2024-11-26)

### Clap Version

version = "4.5.21"

### Minimal reproducible code

main.rs root crate

```rust
fn main() {
    sub::process_args();

    println!("Hello, world!");
}
```

Cargo.toml root crate

```toml
[package]
name = "clap-test"
description = "The main crate in the workspace"
version = "0.1.0"
edition = "2021"

[workspace]
members = [
    "sub"
]

[dependencies]
sub = { version = "0.1.0", package = "sub", path = "sub" }
```

lib.rs sub crate

```rust
use clap::Parser;

#[derive(Parser, Debug, Clone)]
#[command(
    author,
    about,
    long_about = None
)]
/// List of possible command line arguments
pub struct Args {}

pub fn process_args() {
    Args::parse();
}
```

Cargo.toml sub crate

```toml
[package]
name = "sub"
description = "The sub crate in the workspace"
version = "0.1.0"
edition = "2021"

[dependencies]
clap = { version = "~4", features = ["derive"] }
```

Full minimal test crate can be found at https://github.com/lexi-the-cute/clap-test

### Steps to reproduce the bug with the above code

cargo run -- --help

### Actual Behaviour

```
$ cargo run -- --help
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/clap-test --help`
The sub crate in the workspace

Usage: clap-test

Options:
  -h, --help  Print help
```

### Expected Behaviour

The output should follow the same convention as the name of the root crate listed in the Usage line. It should display the description from `clap-test` and not `sub`

```
$ cargo run -- --help
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/clap-test --help`
The main crate in the workspace

Usage: clap-test

Options:
  -h, --help  Print help
```

### Additional Context

The issue can be worked around with the below code

```rust
#[command(
    author,
    about="The main crate in the workspace",
    long_about = None
)]
```

### Debug Output

```
$ cargo run -- --help
    Blocking waiting for file lock on build directory
   Compiling adler2 v2.0.0
   Compiling gimli v0.31.1
   Compiling memchr v2.7.4
   Compiling libc v0.2.167
   Compiling rustc-demangle v0.1.24
   Compiling cfg-if v1.0.0
   Compiling miniz_oxide v0.8.0
   Compiling object v0.36.5
   Compiling addr2line v0.24.2
   Compiling backtrace v0.3.74
   Compiling clap_builder v4.5.21
   Compiling clap v4.5.21
   Compiling sub v0.1.0 (/home/alexis/Documents/software/clap-test/sub)
   Compiling clap-test v0.1.0 (/home/alexis/Documents/software/clap-test)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 5.85s
     Running `target/debug/clap-test --help`
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="sub"
[clap_builder::builder::command]Command::_propagate:sub
[clap_builder::builder::command]Command::_check_help_and_version:sub expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:sub
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
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
[clap_builder::builder::command]Command::write_help_err: sub, use_long=false
[clap_builder::builder::command]Command::long_help_exists: false
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=sub, use_long=false
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
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_no_title: usage=clap-test
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
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
The sub crate in the workspace

Usage: clap-test

Options:
  -h, --help  Print help
```

---

_Label `C-bug` added by @lexi-the-cute on 2024-12-02 09:18_

---

_Comment by @epage on 2024-12-02 16:00_

This is not a bug but expected behavior.  The only way for us to get the description is from Cargo and Cargo very intentionally only tells you about your own package and not the packages that depend on you.

You can workaround this by
- Having the `lib` expose the `Args` and flattening it in an `Args` in your bin
- Having the `lib` accept a parameter and manually create the `Command` from `Args` to pass it in
- Hand write the `about` as you said.

---

_Closed by @epage on 2024-12-02 16:00_

---
