---
number: 5915
title: "`hide_long_help(true)` causes help alignment issues"
type: issue
state: closed
author: wmmc88
labels:
  - C-bug
assignees: []
created_at: 2025-02-18T23:51:37Z
updated_at: 2025-02-19T15:25:05Z
url: https://github.com/clap-rs/clap/issues/5915
synced_at: 2026-01-10T01:28:19Z
---

# `hide_long_help(true)` causes help alignment issues

---

_Issue opened by @wmmc88 on 2025-02-18 23:51_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.86.0-nightly (854f22563 2025-01-31)

### Clap Version

4.5.30

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser)]
struct Cli {
    #[arg(long)]
    pub test_arg: Option<String>,

    #[arg(long, hide_long_help(true))] // false fixes the issue of newlines
    /// Process all packages in the workspace
    pub all: bool,
}

fn main() {
    Cli::parse();
}
```

`Cargo.toml`
```toml
[dependencies]
clap = { version = "4.5.30", features = ["derive"] }
```

### Steps to reproduce the bug with the above code

`cargo run -- --help`

### Actual Behaviour

The output looks like:
```
cargo run -- --help
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.05s
     Running `target\debug\bar.exe --help`
Usage: bar.exe [OPTIONS]

Options:
      --test-arg <TEST_ARG>


  -h, --help
          Print help (see a summary with '-h')
```

Notice that the help text is on a new line.

### Expected Behaviour

When changing `hide_long_help` to `false`, the output looks like:
```
cargo run -- --help
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.06s
     Running `target\debug\bar.exe --help`
Usage: bar.exe [OPTIONS]

Options:
      --test-arg <TEST_ARG>
      --all                  Process all packages in the workspace
  -h, --help                 Print help
```

I expect that adding `hide_long_help` to an arg to not mess up the help text and put all the help text in new lines

### Additional Context

I originally ran into this issue while using `clap_cargo::Workspace` but was able to narrow the issue down to the args that have `hide_short_help(true)` or `hide_long_help(true)`.

### Debug Output

```
cargo run -- --help
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.29s
     Running `target\debug\bar.exe --help`
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="bar"
[clap_builder::builder::command]Command::_propagate:bar
[clap_builder::builder::command]Command::_check_help_and_version:bar expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:bar
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:test_arg
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:all
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
[clap_builder::builder::command]Command::long_help_exists: true
[clap_builder::builder::command]Command::write_help_err: bar, use_long=true
[clap_builder::builder::command]Command::long_help_exists: true
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=bar, use_long=true
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=test_arg
[clap_builder::output::help_template]HelpTemplate::write_templated_help
[clap_builder::output::help_template]HelpTemplate::write_before_help
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=test_arg
[clap_builder::builder::command]Command::groups_for_arg: id="test_arg"
[ clap_builder::output::usage]Usage::needs_options_tag:iter:iter: grp_s="Cli"
[ clap_builder::output::usage]Usage::needs_options_tag:iter: [OPTIONS] required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_no_title: usage=bar.exe [OPTIONS]
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=test_arg
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=all
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args Options
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=test_arg
[clap_builder::output::help_template]HelpTemplate::write_args: arg="test_arg" longest=21
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args: arg="help" longest=21
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=test_arg
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--test-arg <TEST_ARG>
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--test-arg <TEST_ARG>
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=test_arg, next_line_help=true, longest=21
[clap_builder::output::help_template]HelpTemplate::align_to_about: printing long help so skip alignment
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: Next Line...true
[clap_builder::output::help_template]HelpTemplate::help: help_width=10, spaces=0, avail=90
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=true, longest=21
[clap_builder::output::help_template]HelpTemplate::align_to_about: printing long help so skip alignment
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: Next Line...true
[clap_builder::output::help_template]HelpTemplate::help: help_width=10, spaces=36, avail=90
[clap_builder::output::help_template]HelpTemplate::write_after_help
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
Usage: bar.exe [OPTIONS]

Options:
      --test-arg <TEST_ARG>


  -h, --help
          Print help (see a summary with '-h')
```

With `hide_long_help(false)`:
```
cargo run -- --help
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.06s
     Running `target\debug\bar.exe --help`
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="bar"
[clap_builder::builder::command]Command::_propagate:bar
[clap_builder::builder::command]Command::_check_help_and_version:bar expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:bar
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:test_arg
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:all
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
[clap_builder::builder::command]Command::write_help_err: bar, use_long=false
[clap_builder::builder::command]Command::long_help_exists: false
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=bar, use_long=false
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=test_arg
[clap_builder::output::help_template]HelpTemplate::write_templated_help
[clap_builder::output::help_template]HelpTemplate::write_before_help
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=test_arg
[clap_builder::builder::command]Command::groups_for_arg: id="test_arg"
[ clap_builder::output::usage]Usage::needs_options_tag:iter:iter: grp_s="Cli"
[ clap_builder::output::usage]Usage::needs_options_tag:iter: [OPTIONS] required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_no_title: usage=bar.exe [OPTIONS]
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=test_arg
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=all
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args Options
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=test_arg
[clap_builder::output::help_template]HelpTemplate::write_args: arg="test_arg" longest=21
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=all
[clap_builder::output::help_template]HelpTemplate::write_args: arg="all" longest=21
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args: arg="help" longest=21
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=test_arg
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--test-arg <TEST_ARG>
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=all
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--all
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--test-arg <TEST_ARG>
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=test_arg, next_line_help=false, longest=21
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=21, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=29, spaces=0, avail=71
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--all
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=all, next_line_help=false, longest=21
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=5, spaces=18
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=29, spaces=37, avail=71
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=21
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=6, spaces=17
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=29, spaces=10, avail=71
[clap_builder::output::help_template]HelpTemplate::write_after_help
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
Usage: bar.exe [OPTIONS]

Options:
      --test-arg <TEST_ARG>
      --all                  Process all packages in the workspace
  -h, --help                 Print help
```


---

_Label `C-bug` added by @wmmc88 on 2025-02-18 23:51_

---

_Renamed from "hide_long_help(true) causes help alignment issues" to "`hide_long_help(true)` causes help alignment issues" by @wmmc88 on 2025-02-18 23:51_

---

_Comment by @epage on 2025-02-19 14:10_

This isn't help alignment issue but that "next line help" is automatically used for long help (`--help`).  If you ran short help (`-h`) you wouldn't see this.

The logic we use to determine whether long help is present:
https://github.com/clap-rs/clap/blob/ac93ac6fa90be18106e31074e25968fe4d4f1967/clap_builder/src/builder/command.rs#L5066-L5089

I believe the intention here is that if you say you only want to hide something in the long help, then that makes the long help different than the short help and so the user intends for it to be shown separately.  The long help then always uses next-line help.

> I originally ran into this issue while using clap_cargo::Workspace but was able to narrow the issue down to the args that have hide_short_help(true) or hide_long_help(true).

`clap_cargo` should instead be using `hide = true`

---

_Referenced in [crate-ci/clap-cargo#83](../../crate-ci/clap-cargo/pulls/83.md) on 2025-02-19 14:13_

---

_Comment by @wmmc88 on 2025-02-19 15:04_

I'm not sure I understand why if a single arg disables long or short help, all other args long help uses "next line help". Based on the logic, even when short help is hidden for a single arg, it causes all the args long help to be on newlines..

But thanks for the clap_cargo fix!

---

_Comment by @epage on 2025-02-19 15:25_

Long help assumes there will be multi-paragraph descriptions for the command and arguments.  It therefore always uses next-line-help to make this more readable.  Whether to do that or not or allow it to be overriden is separate from this.  As the long help detection is working as expected and clap-cargo is fixed, I'm going to go ahead and close this.

---

_Closed by @epage on 2025-02-19 15:25_

---
