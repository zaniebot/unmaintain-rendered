---
number: 4772
title: "`term_width` docs on disabled `wrap_help` seem wrong"
type: issue
state: closed
author: CosmicHorrorDev
labels:
  - C-bug
assignees: []
created_at: 2023-03-18T17:49:07Z
updated_at: 2023-03-20T14:37:04Z
url: https://github.com/clap-rs/clap/issues/4772
synced_at: 2026-01-10T01:28:01Z
---

# `term_width` docs on disabled `wrap_help` seem wrong

---

_Issue opened by @CosmicHorrorDev on 2023-03-18 17:49_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.68.0 (2c8cc3432 2023-03-06)

### Clap Version

v4.1.10

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser)]
struct NoWrap {
    /// Wow this is some long help text. Would sure be nice if this wrapped automatically for us.
    /// Things would get really hard to read otherwise
    value: String,
}

fn main() {
    let _ = NoWrap::parse();
}
```

### Steps to reproduce the bug with the above code

```
$ cargo run -- --help
```

### Actual Behaviour

```
Usage: repro <VALUE>

Arguments:
  <VALUE>  Wow this is some long help text. Would sure be nice if this wrapped automatically for us. Things would get really hard to read otherwise

Options:
  -h, --help  Print help
```

### Expected Behaviour

```
Usage: repro <VALUE>

Arguments:
  <VALUE>  Wow this is some long help text. Would sure be nice if this wrapped automatically for us.
           Things would get really hard to read otherwise

Options:
  -h, --help  Print help
```

### Additional Context

The docs for [`Command::term_width()`](https://docs.rs/clap/4.1.8/clap/builder/struct.Command.html#method.term_width) include (emphasis mine)

> Defaults to current terminal width when `wrap_help` feature flag is enabled. **If the flag is disabled or it cannot be determined, the default is 100.**

From my understanding that means that if I don't have `wrap_help` enabled then the terminal width will be considered 100: however, it seems that the default in this case is 0 instead aka source formatting

[This discussion](https://github.com/clap-rs/clap/discussions/4647) appears to cover the same issue, but doesn't bring up the misleading docs

### Debug Output

```
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="repro"
[      clap::builder::command]  Command::_propagate:repro
[      clap::builder::command]  Command::_check_help_and_version:repro expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_propagate_global_args:repro
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:value
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
[      clap::builder::command]  Command::write_help_err: repro, use_long=false
[      clap::builder::command]  Command::long_help_exists: false
[          clap::output::help]  write_help
[ clap::output::help_template]  HelpTemplate::new cmd=repro, use_long=false
[ clap::output::help_template]  should_show_arg: use_long=false, arg=value
[ clap::output::help_template]  should_show_arg: use_long=false, arg=help
[ clap::output::help_template]  HelpTemplate::write_templated_help
[ clap::output::help_template]  HelpTemplate::write_before_help
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::needs_options_tag
[         clap::output::usage]  Usage::needs_options_tag:iter: f=help
[         clap::output::usage]  Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage]  Usage::needs_options_tag: [OPTIONS] not required
[         clap::output::usage]  Usage::get_args: incls=[]
[         clap::output::usage]  Usage::get_args: unrolled_reqs=["value"]
[         clap::output::usage]  Usage::get_args: ret_val=[StyledStr { pieces: [(Some(Placeholder), "<VALUE>")] }]
[         clap::output::usage]  Usage::create_help_usage: usage=repro <VALUE>
[ clap::output::help_template]  HelpTemplate::write_all_args
[ clap::output::help_template]  should_show_arg: use_long=false, arg=value
[ clap::output::help_template]  should_show_arg: use_long=false, arg=help
[ clap::output::help_template]  HelpTemplate::write_args Arguments
[ clap::output::help_template]  should_show_arg: use_long=false, arg=value
[ clap::output::help_template]  HelpTemplate::write_args: arg="value" longest=7
[ clap::output::help_template]  should_show_arg: use_long=false, arg=value
[ clap::output::help_template]  HelpTemplate::spec_vals: a=<VALUE>
[ clap::output::help_template]  HelpTemplate::spec_vals: a=<VALUE>
[ clap::output::help_template]  HelpTemplate::short
[ clap::output::help_template]  HelpTemplate::long
[ clap::output::help_template]  HelpTemplate::align_to_about: arg=value, next_line_help=false, longest=7
[ clap::output::help_template]  HelpTemplate::align_to_about: positional=true arg_len=7, spaces=2
[ clap::output::help_template]  HelpTemplate::help
[ clap::output::help_template]  HelpTemplate::help: help_width=11, spaces=136, avail=89
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
[ clap::output::help_template]  HelpTemplate::help: help_width=14, spaces=10, avail=86
[ clap::output::help_template]  HelpTemplate::write_after_help
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
Usage: repro <VALUE>

Arguments:
  <VALUE>  Wow this is some long help text. Would sure be nice if this wrapped automatically for us. Things would get really hard to read otherwise

Options:
  -h, --help  Print help
```

---

_Label `C-bug` added by @CosmicHorrorDev on 2023-03-18 17:49_

---

_Comment by @epage on 2023-03-20 14:37_

Thanks for pointing out the stale documentation, it was updated in cc1474f

---

_Closed by @epage on 2023-03-20 14:37_

---
