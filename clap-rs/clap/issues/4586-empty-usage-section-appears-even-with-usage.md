---
number: 4586
title: "Empty \"Usage: \" section appears even with usage feature flag disabled"
type: issue
state: closed
author: mgunyho
labels:
  - C-bug
  - A-help
  - S-wont-fix
assignees: []
created_at: 2022-12-29T16:12:19Z
updated_at: 2022-12-29T16:54:26Z
url: https://github.com/clap-rs/clap/issues/4586
synced_at: 2026-01-10T01:27:58Z
---

# Empty "Usage: " section appears even with usage feature flag disabled

---

_Issue opened by @mgunyho on 2022-12-29 16:12_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0 (a55dd71d5 2022-09-19)

### Clap Version

4.0.32

### Minimal reproducible code

`main.rs`:
```rust
fn main() {
    clap::Command::new("test")
        .get_matches();
}
```

`Cargo.toml`:
```toml
[package]
name = "clap-empty-usage"
version = "0.1.0"
edition = "2021"

[dependencies.clap]
version = "4"
default-features = false
features = ["help", "std"]
```

### Steps to reproduce the bug with the above code

```shell
cargo run -- --help
```

### Actual Behaviour

The program prints
```
Usage: 

Options:
  -h, --help  Print help information
```
(Note the trailing space after `Usage: `)

### Expected Behaviour

With the `usage` feature-flag disabled, I would have expected no `Usage: ` line, just
```
Options:
  -h, --help  Print help information
```

### Additional Context

I suppose the `Usage: ` line shouldn't be there at all if the feature flag is missing, right? Especially the trailing space made me suspect it's a bug.

I noticed this when migrating from clap to v4 from v3 in my app (which has `default-features = false`). I actually wanted the usage line, but I hadn't added the `usage` feature flag (I was reading the migration guide [here](https://github.com/clap-rs/clap/releases/tag/v4.0.0-rc.1), which didn't include instructions to update the feature flags).

### Debug Output

```
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="test"
[      clap::builder::command]  Command::_propagate:test
[      clap::builder::command]  Command::_check_help_and_version:test expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_propagate_global_args:test
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("--help")' ([45, 45, 104, 101, 108, 112])
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("--help")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::parse_long_arg
[        clap::parser::parser]  Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser]  Parser::parse_long_arg: Found valid arg or flag '--help'
[        clap::parser::parser]  Parser::parse_long_arg("help"): Presence validated
[        clap::parser::parser]  Parser::react action=Help, identifier=Some(Long), source=CommandLine
[        clap::parser::parser]  Help: use_long=true
[      clap::builder::command]  Command::long_help_exists: false
[      clap::builder::command]  Command::write_help_err: test, use_long=false
[      clap::builder::command]  Command::long_help_exists: false
[          clap::output::help]  write_help
[ clap::output::help_template]  HelpTemplate::new cmd=test, use_long=false
[ clap::output::help_template]  should_show_arg: use_long=false, arg=help
[ clap::output::help_template]  HelpTemplate::write_templated_help
[ clap::output::help_template]  HelpTemplate::write_before_help
[         clap::output::usage]  Usage::create_usage_no_title
[ clap::output::help_template]  HelpTemplate::write_all_args
[ clap::output::help_template]  should_show_arg: use_long=false, arg=help
[ clap::output::help_template]  HelpTemplate::write_args Options
[ clap::output::help_template]  should_show_arg: use_long=false, arg=help
[ clap::output::help_template]  HelpTemplate::write_args: arg="help" longest=6
[ clap::output::help_template]  should_show_arg: use_long=false, arg=help
[ clap::output::help_template]  HelpTemplate::spec_vals: a=--help
[ clap::output::help_template]  HelpTemplate::spec_vals: a=--help
[ clap::output::help_template]  HelpTemplate::short
[ clap::output::help_template]  HelpTemplate::long
[ clap::output::help_template]  HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=6
[ clap::output::help_template]  HelpTemplate::align_to_about: positional=false arg_len=6, spaces=2
[ clap::output::help_template]  HelpTemplate::help
[ clap::output::help_template]  HelpTemplate::help: help_width=14, spaces=22, avail=86
[ clap::output::help_template]  HelpTemplate::write_after_help
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Command::color: Color setting...
```

---

_Label `C-bug` added by @mgunyho on 2022-12-29 16:12_

---

_Label `A-help` added by @epage on 2022-12-29 16:54_

---

_Label `S-wont-fix` added by @epage on 2022-12-29 16:54_

---

_Comment by @epage on 2022-12-29 16:54_

We do not expect to address this.  When disabling the usage feature, the user will need to either manually override the usage or change the help template.  This is an advanced enough of a scenario that the cost (including binary size) is not justifiable.

---

_Closed by @epage on 2022-12-29 16:54_

---
