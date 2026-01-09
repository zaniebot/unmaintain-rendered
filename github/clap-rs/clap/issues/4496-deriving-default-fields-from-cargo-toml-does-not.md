---
number: 4496
title: Deriving default fields from Cargo.toml does not work
type: issue
state: closed
author: laclouis5
labels:
  - C-bug
assignees: []
created_at: 2022-11-21T12:04:23Z
updated_at: 2022-11-21T12:36:02Z
url: https://github.com/clap-rs/clap/issues/4496
synced_at: 2026-01-07T13:12:20-06:00
---

# Deriving default fields from Cargo.toml does not work

---

_Issue opened by @laclouis5 on 2022-11-21 12:04_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.65.0 (897e37553 2022-11-02)

### Clap Version

4.0.26

### Minimal reproducible code

Theo code is the one from the ["Configuring the Parser"](https://docs.rs/clap/latest/clap/_derive/_tutorial/index.html#configuring-the-parser) example, the part about using `#[command(author, version, about)]` to default attributes from fields in Cargo.toml:

```rust
use clap::Parser;

#[derive(Parser)]
#[command(author, version, about, long_about = None)] // Read from `Cargo.toml`
struct Cli {
    #[arg(long)]
    two: String,
    #[arg(long)]
    one: String,
}

fn main() {
    let cli = Cli::parse();

    println!("two: {:?}", cli.two);
    println!("one: {:?}", cli.one);
}
```


### Steps to reproduce the bug with the above code

```
cargo new clapbug
cd clapbug
cargo add clap --features derive
```

Add a "about" field to Cargo.toml:

```toml
[package]
name = "clapbug"
version = "0.1.0"
edition = "2021"
about = "A simple to use, efficient, and full-featured Command Line Argument Parser"

[dependencies]
clap = { version = "4.0.26", features = ["derive"] }
```

Run:

```
cargo run -- --help
```

### Actual Behaviour

The about message is not shown. Output:

```
Usage: clapbug --two <TWO> --one <ONE>

Options:
      --two <TWO>  
      --one <ONE>  
  -h, --help       Print help information
  -V, --version    Print version information
```



### Expected Behaviour

The expected output is the one from the example:

```
A simple to use, efficient, and full-featured Command Line Argument Parser

Usage: clapbug --two <TWO> --one <ONE>

Options:
      --two <TWO>  
      --one <ONE>  
  -h, --help       Print help information
  -V, --version    Print version information
```

### Additional Context

The compiler outputs a warning `warning: unused manifest key: package.about`.

### Debug Output

```
[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="clapbug"
[      clap::builder::command] 	Command::_propagate:clapbug
[      clap::builder::command] 	Command::_check_help_and_version:clapbug expand_help_tree=false
[      clap::builder::command] 	Command::long_help_exists
[      clap::builder::command] 	Command::_check_help_and_version: Building default --help
[      clap::builder::command] 	Command::_check_help_and_version: Building default --version
[      clap::builder::command] 	Command::_propagate_global_args:clapbug
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:two
[clap::builder::debug_asserts] 	Arg::_debug_asserts:one
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Arg::_debug_asserts:version
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--help")' ([45, 45, 104, 101, 108, 112])
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("--help")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::parse_long_arg
[        clap::parser::parser] 	Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser] 	Parser::parse_long_arg: Found valid arg or flag '--help'
[        clap::parser::parser] 	Parser::parse_long_arg("help"): Presence validated
[        clap::parser::parser] 	Parser::react action=Help, identifier=Some(Long), source=CommandLine
[        clap::parser::parser] 	Help: use_long=true
[      clap::builder::command] 	Command::long_help_exists: false
[      clap::builder::command] 	Command::write_help_err: clapbug, use_long=false
[      clap::builder::command] 	Command::long_help_exists: false
[          clap::output::help] 	write_help
[ clap::output::help_template] 	HelpTemplate::new cmd=clapbug, use_long=false
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=two
[ clap::output::help_template] 	HelpTemplate::write_templated_help
[ clap::output::help_template] 	HelpTemplate::write_before_help
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::needs_options_tag
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=two
[         clap::output::usage] 	Usage::needs_options_tag:iter Option is required
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=one
[         clap::output::usage] 	Usage::needs_options_tag:iter Option is required
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=help
[         clap::output::usage] 	Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=version
[         clap::output::usage] 	Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage] 	Usage::needs_options_tag: [OPTIONS] not required
[         clap::output::usage] 	Usage::get_args: incls=[]
[         clap::output::usage] 	Usage::get_args: unrolled_reqs=["two", "one"]
[         clap::output::usage] 	Usage::get_args: ret_val=[StyledStr { pieces: [(Some(Literal), "--"), (Some(Literal), "two"), (Some(Placeholder), " "), (Some(Placeholder), "<TWO>")] }, StyledStr { pieces: [(Some(Literal), "--"), (Some(Literal), "one"), (Some(Placeholder), " "), (Some(Placeholder), "<ONE>")] }]
[         clap::output::usage] 	Usage::create_help_usage: usage=clapbug --two <TWO> --one <ONE>
[ clap::output::help_template] 	HelpTemplate::write_all_args
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=two
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=one
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=help
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=version
[ clap::output::help_template] 	HelpTemplate::write_args Options
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=two
[ clap::output::help_template] 	HelpTemplate::write_args: arg="two" longest=11
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=one
[ clap::output::help_template] 	HelpTemplate::write_args: arg="one" longest=11
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=help
[ clap::output::help_template] 	HelpTemplate::write_args: arg="help" longest=11
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=version
[ clap::output::help_template] 	HelpTemplate::write_args: arg="version" longest=11
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=two
[ clap::output::help_template] 	HelpTemplate::spec_vals: a=--two <TWO>
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=one
[ clap::output::help_template] 	HelpTemplate::spec_vals: a=--one <ONE>
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=help
[ clap::output::help_template] 	HelpTemplate::spec_vals: a=--help
[ clap::output::help_template] 	should_show_arg: use_long=false, arg=version
[ clap::output::help_template] 	HelpTemplate::spec_vals: a=--version
[ clap::output::help_template] 	HelpTemplate::spec_vals: a=--two <TWO>
[ clap::output::help_template] 	HelpTemplate::short
[ clap::output::help_template] 	HelpTemplate::long
[ clap::output::help_template] 	HelpTemplate::align_to_about: arg=two, next_line_help=false, longest=11
[ clap::output::help_template] 	HelpTemplate::align_to_about: positional=false arg_len=11, spaces=2
[ clap::output::help_template] 	HelpTemplate::help
[ clap::output::help_template] 	HelpTemplate::help: help_width=19, spaces=0, avail=81
[ clap::output::help_template] 	HelpTemplate::spec_vals: a=--one <ONE>
[ clap::output::help_template] 	HelpTemplate::short
[ clap::output::help_template] 	HelpTemplate::long
[ clap::output::help_template] 	HelpTemplate::align_to_about: arg=one, next_line_help=false, longest=11
[ clap::output::help_template] 	HelpTemplate::align_to_about: positional=false arg_len=11, spaces=2
[ clap::output::help_template] 	HelpTemplate::help
[ clap::output::help_template] 	HelpTemplate::help: help_width=19, spaces=0, avail=81
[ clap::output::help_template] 	HelpTemplate::spec_vals: a=--help
[ clap::output::help_template] 	HelpTemplate::short
[ clap::output::help_template] 	HelpTemplate::long
[ clap::output::help_template] 	HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=11
[ clap::output::help_template] 	HelpTemplate::align_to_about: positional=false arg_len=6, spaces=7
[ clap::output::help_template] 	HelpTemplate::help
[ clap::output::help_template] 	HelpTemplate::help: help_width=19, spaces=22, avail=81
[ clap::output::help_template] 	HelpTemplate::spec_vals: a=--version
[ clap::output::help_template] 	HelpTemplate::short
[ clap::output::help_template] 	HelpTemplate::long
[ clap::output::help_template] 	HelpTemplate::align_to_about: arg=version, next_line_help=false, longest=11
[ clap::output::help_template] 	HelpTemplate::align_to_about: positional=false arg_len=9, spaces=4
[ clap::output::help_template] 	HelpTemplate::help
[ clap::output::help_template] 	HelpTemplate::help: help_width=19, spaces=25, avail=81
[ clap::output::help_template] 	HelpTemplate::write_after_help
[      clap::builder::command] 	Command::color: Color setting...
[      clap::builder::command] 	Auto
[      clap::builder::command] 	Command::color: Color setting...
[      clap::builder::command] 	Auto
```

---

_Label `C-bug` added by @laclouis5 on 2022-11-21 12:04_

---

_Comment by @epage on 2022-11-21 12:36_

The manifest field that is being read is `package.description`, see https://doc.rust-lang.org/cargo/reference/manifest.html#the-description-field.

---

_Closed by @epage on 2022-11-21 12:36_

---

_Referenced in [clap-rs/clap#4776](../../clap-rs/clap/issues/4776.md) on 2023-03-22 04:42_

---
