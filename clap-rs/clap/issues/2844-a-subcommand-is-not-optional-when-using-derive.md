```yaml
number: 2844
title: A subcommand is not optional when using derive macros
type: issue
state: closed
author: tobx
labels:
  - C-bug
assignees: []
created_at: 2021-10-11T12:35:44Z
updated_at: 2021-10-11T14:05:17Z
url: https://github.com/clap-rs/clap/issues/2844
synced_at: 2026-01-12T16:14:13Z
```

# A subcommand is not optional when using derive macros

---

_@tobx_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

3.0.0-beta.4

### Minimal reproducible code

```rust
use clap::Clap;

#[derive(Clap)]
#[clap(name = "test")]
struct Opts {
    #[clap(subcommand)]
    subcmd: SubCommand,
}

#[derive(Clap)]
enum SubCommand {
    Test(Test),
}

#[derive(Clap)]
struct Test;

fn main() {
    Opts::parse();
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

Running the app without arguments shows the help text and exits.

The help text shows the subcommand as required:

```
USAGE:
    test-clap <SUBCOMMAND>
```

### Expected Behaviour

The subcommand should be optional.

The help text should show the subcommand as optional:

```
USAGE:
    test-clap [SUBCOMMAND]
```

### Additional Context

This minimal example without using derive macros works as expected:

```rust
use clap::App;

fn main() {
    App::new("test")
        .subcommand(App::new("test"))
        .get_matches();
}
```

### Debug Output

[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:test
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Building help subcommand
[            clap::build::app] 	App::_propagate_global_args:test
[            clap::build::app] 	App::_derive_display_order:test
[            clap::build::app] 	App::_derive_display_order:test
[            clap::build::app] 	App::_derive_display_order:help
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with: SubcommandRequiredElseHelp=true
[         clap::parse::parser] 	Parser::color_help
[          clap::output::help] 	Help::new
[          clap::output::help] 	Help::write_help
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::write_templated_help
[          clap::output::help] 	Help::write_before_help
[          clap::output::help] 	Help::write_bin_name
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs=[]
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage] 	Usage::needs_flags_tag
[         clap::output::usage] 	Usage::needs_flags_tag:iter: f=help
[         clap::output::usage] 	Usage::needs_flags_tag:iter: f=version
[         clap::output::usage] 	Usage::needs_flags_tag: [FLAGS] not required
[         clap::output::usage] 	Usage::create_help_usage: usage=test-clap <SUBCOMMAND>
[          clap::output::help] 	Help::write_all_args
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	should_show_arg: use_long=false, arg=version
[          clap::output::help] 	Help::write_args
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::write_args: Current Longest...2
[          clap::output::help] 	Help::write_args: New Longest...6
[          clap::output::help] 	should_show_arg: use_long=false, arg=version
[          clap::output::help] 	Help::write_args: Current Longest...6
[          clap::output::help] 	Help::write_args: New Longest...9
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::spec_vals: a=--help
[          clap::output::help] 	should_show_arg: use_long=false, arg=version
[          clap::output::help] 	Help::spec_vals: a=--version
[          clap::output::help] 	Help::spec_vals: a=--help
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=help
[          clap::output::help] 	Help::val: Has switch...
[          clap::output::help] 	Yes
[          clap::output::help] 	Help::val: nlh...false
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::spec_vals: a=--version
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=version
[          clap::output::help] 	Help::val: Has switch...
[          clap::output::help] 	Yes
[          clap::output::help] 	Help::val: nlh...false
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_subcommands
[          clap::output::help] 	Help::write_subcommands longest = 4
[          clap::output::help] 	Help::sc_spec_vals: a=test
[          clap::output::help] 	Help::sc_spec_vals: a=help
[          clap::output::help] 	Help::write_subcommand
[          clap::output::help] 	Help::sc_spec_vals: a=help
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_subcommand
[          clap::output::help] 	Help::sc_spec_vals: a=test
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_after_help
[           clap::output::fmt] 	is_a_tty: stderr=true

---

_Label `T: bug` added by @tobx on 2021-10-11 12:35_

---

_Comment by @epage on 2021-10-11 13:33_

I've added an arg to your example:
```rust
use clap::Clap;

#[derive(Clap)]
#[clap(name = "test")]
struct Opts {
    #[clap(subcommand)]
    subcmd: SubCommand,
}

#[derive(Clap)]
enum SubCommand {
    Test(Test),
}

#[derive(Clap)]
struct Test {
    #[clap(long)]
    arg: String,
}

fn main() {
    Opts::parse();
}
```
In this, both `--arg` and the subcommand are required.  As-represented, there is no alternative; for us to construct an `Opts`; we have to populate the `subcmd` and `arg` fields with something.

What you can do is:
```rust
use clap::Clap;

#[derive(Clap)]
#[clap(name = "test")]
struct Opts {
    #[clap(subcommand)]
    subcmd: Option<SubCommand>,
}

#[derive(Clap)]
enum SubCommand {
    Test(Test),
}

#[derive(Clap)]
struct Test {
    #[clap(long)]
    arg: Option<String>,
}

fn main() {
    Opts::parse();
}
```
Now `Opts` can be constructed without the user selecting a subcmd or an arg.  `clap_derive` takes advantage of this to use it as a way of specifying an argument is not required.

There is another way of allowing non-required arguments, but not subcommands, defaults
```rust
#[derive(Clap)]
struct Test {
    #[clap(long, default_value_t)]
    arg: String,
}
```

---

_Comment by @tobx on 2021-10-11 13:37_

I just tried if I can just set `Option<SubCommand>` and wanted to close the issue, but you were faster, thank you for the detailed answer.

---

_Closed by @tobx on 2021-10-11 13:37_

---

_Locked by @clap-rs on 2021-10-11 14:05_

---
