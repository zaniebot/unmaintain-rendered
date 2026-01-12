```yaml
number: 5529
title: "command! macro doesn't get inherited properties from workspace"
type: issue
state: closed
author: tylerjw
labels:
  - C-bug
assignees: []
created_at: 2024-06-12T15:19:11Z
updated_at: 2024-06-12T21:09:23Z
url: https://github.com/clap-rs/clap/issues/5529
synced_at: 2026-01-12T16:14:17Z
```

# command! macro doesn't get inherited properties from workspace

---

_@tylerjw_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.78.0 (9b00956e5 2024-04-29)

### Clap Version

4.5.7

### Minimal reproducible code

```rust
use clap::command;

fn main() {
    let _matches = command!()
        .get_matches();
}
```

workspace Cargo.toml
```toml
[workspace]
members = [
    "test",
]

[workspace.package]
authors = ["me"]
```

test/Cargo.toml
```toml
[package]
name = "min_test"
authors.workspace = true

[dependencies]
clap = { version = "4.5.7", features = ["cargo"] }
```

### Steps to reproduce the bug with the above code

```bash
cargo run --bin min_test -- --help
```

### Actual Behaviour

```
Usage: min_test

Options:
  -h, --help     Print help
  -V, --version  Print version
```

### Expected Behaviour

```
me

Usage: min_test

Options:
  -h, --help     Print help
  -V, --version  Print version
```

### Additional Context

_No response_

### Debug Output

```bash
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="min_test"
[clap_builder::builder::command]Command::_propagate:min_test
[clap_builder::builder::command]Command::_check_help_and_version:min_test expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_propagate_global_args:min_test
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
[clap_builder::builder::command]Command::write_help_err: min_test, use_long=false
[clap_builder::builder::command]Command::long_help_exists: false
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=min_test, use_long=false
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
[ clap_builder::output::usage]Usage::create_usage_no_title: usage=min_test
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=version
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

_Label `C-bug` added by @tylerjw on 2024-06-12 15:19_

---

_Comment by @epage on 2024-06-12 15:29_

I expect if you set `package.authors`, you would get the same result.

In clap [v4](https://github.com/clap-rs/clap/blob/master/CHANGELOG.md#400---2022-09-28), we dropped author from the [default help template](https://docs.rs/crate/clap_builder/latest/source/src/output/help_template.rs)

You can add it back with a [custom help template](https://docs.rs/clap/latest/clap/struct.Command.html#method.help_template).

---

_Comment by @tylerjw on 2024-06-12 18:09_

Oh, thank you for the reply. I was migrating an old project from 2 to 4 and found this.

---

_Closed by @tylerjw on 2024-06-12 18:09_

---

_Comment by @tylerjw on 2024-06-12 19:16_

I don't know if this is a bug or not but when I try to use the same feature via derive it doesn't seem to work (output is missing author):

```rust
const HELP_TEMPLATE: &str = "\
{before-help}{name} {version}
{author-with-newline}{about-with-newline}
{usage-heading} {usage}

{all-args}{after-help}";

#[derive(Parser, Debug)]
#[command(version, about, help_template = HELP_TEMPLATE)]
```

---

_Comment by @epage on 2024-06-12 19:21_

`author` attribute is missing.

The following works for me
```rust
#!/usr/bin/env nargo
---
[package]
description = "some description"
authors = ["me"]

[dependencies]
clap = { version = "4", features = ["derive"] }
---

use clap::Parser;

const HELP_TEMPLATE: &str = "\
{before-help}{name} {version}
{author-with-newline}{about-with-newline}
{usage-heading} {usage}

{all-args}{after-help}";

#[derive(Parser, Debug)]
#[command(author, version, about, help_template = HELP_TEMPLATE)]
struct Cli {
}

fn main() {
    Cli::parse();
}

```

---

_Comment by @tylerjw on 2024-06-12 20:50_

Thank you again; I didn't understand that I needed the author attribute.

---

_Comment by @epage on 2024-06-12 21:09_

Yeah, structopt required `no_author` and such to opt-out.  We switched to explicit opt-in.

btw for 3.0.0 and 4.0.0 we had migration guides (e.g. [3.0.0](https://github.com/clap-rs/clap/blob/master/CHANGELOG.md#migrating-1) and specifically called out subtle breaking changes like this, e.g.

> From structopt 0.3.25
>
> - By default, the App isn't initialized with crate information anymore. Now opt-in via #[clap(author)], #[clap(about)], #[clap(version)] (https://github.com/clap-rs/clap/issues/3034)

---
