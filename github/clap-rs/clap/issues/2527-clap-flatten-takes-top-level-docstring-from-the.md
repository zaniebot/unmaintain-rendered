---
number: 2527
title: "`#[clap(flatten)]` takes top-level docstring from the flattened struct, not the struct being flattened into"
type: issue
state: closed
author: glittershark
labels:
  - C-bug
  - A-derive
  - ":money_with_wings: $10"
assignees: []
created_at: 2021-06-08T13:48:33Z
updated_at: 2021-08-12T20:34:49Z
url: https://github.com/clap-rs/clap/issues/2527
synced_at: 2026-01-07T13:12:19-06:00
---

# `#[clap(flatten)]` takes top-level docstring from the flattened struct, not the struct being flattened into

---

_Issue opened by @glittershark on 2021-06-08 13:48_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.52.0-nightly (e37a13cc3 2021-02-28)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

```rust
use clap::Clap;

/// this is the docstring for Flattened
#[derive(Clap)]
struct Flattened {
    #[clap(long)]
    foo: bool,
}

/// this is the docstring for Command
#[derive(Clap)]
struct Command {
    #[clap(flatten)]
    flattened: Flattened,
}

fn main() {
    Command::parse();
}
```


### Steps to reproduce the bug with the above code

```
cargo run -- --help
```

### Actual Behaviour

```
my-binary
this is the docstring for Flattened

USAGE:
    my-binary [FLAGS]

FLAGS:
        --foo
    -h, --help       Prints help information
    -V, --version    Prints version information
```

### Expected Behaviour

```
my-binary
this is the docstring for Command

USAGE:
    my-binary [FLAGS]

FLAGS:
        --foo
    -h, --help       Prints help information
    -V, --version    Prints version information
```

### Additional Context

It's *possible* I suppose that this is intentional, but it really seems like surprising behavior to me - if it *is* intentional, I'd love the option to turn it off so I can still add a (developer-facing) docstring to Flattened.

### Debug Output

```
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:my-binary
[            clap::build::app] 	App::_derive_display_order:my-binary
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[clap::build::app::debug_asserts] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:foo
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing '"--help"' ([45, 45, 104, 101, 108, 112])
[         clap::parse::parser] 	Parser::is_new_arg: "--help":NotFound
[         clap::parse::parser] 	Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser] 	Parser::is_new_arg: -- found
[         clap::parse::parser] 	Parser::is_new_arg: starts_new_arg=true
[         clap::parse::parser] 	Parser::possible_subcommand: arg="--help"
[         clap::parse::parser] 	Parser::get_matches_with: possible_sc=false, sc=None
[         clap::parse::parser] 	Parser::parse_long_arg
[         clap::parse::parser] 	Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser] 	No
[         clap::parse::parser] 	Parser::parse_long_arg: Found valid opt or flag '--help'
[         clap::parse::parser] 	Parser::check_for_help_and_version_str
[         clap::parse::parser] 	Parser::check_for_help_and_version_str: Checking if --"help" is help or version...
[         clap::parse::parser] 	Help
[         clap::parse::parser] 	[         clap::parse::parser] 	Parser::use_long_help
Parser::help_err: use_long=false
[         clap::parse::parser] 	Parser::use_long_help
[         clap::parse::parser] 	Parser::color_help
[          clap::output::help] 	Help::new
[          clap::output::help] 	Help::write_help
[          clap::output::help] 	Help::write_templated_help
[          clap::output::help] 	Help::write_bin_name
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs=[]
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage] 	Usage::needs_flags_tag
[         clap::output::usage] 	Usage::needs_flags_tag:iter: f=foo
[            clap::build::app] 	App::groups_for_arg: id=foo
[         clap::output::usage] 	Usage::needs_flags_tag:iter: [FLAGS] required
[         clap::output::usage] 	Usage::create_help_usage: usage=my-binary [FLAGS]
[          clap::output::help] 	Help::write_all_args
[          clap::output::help] 	Help::write_args
[          clap::output::help] 	should_show_arg: use_long=false, arg=foo
[          clap::output::help] 	Help::write_args: Current Longest...2
[          clap::output::help] 	Help::write_args: New Longest...5
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::write_args: Current Longest...5
[          clap::output::help] 	Help::write_args: New Longest...6
[          clap::output::help] 	should_show_arg: use_long=false, arg=version
[          clap::output::help] 	Help::write_args: Current Longest...6
[          clap::output::help] 	Help::write_args: New Longest...9
[          clap::output::help] 	Help::write_arg
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=foo
[          clap::output::help] 	Help::spec_vals: a=--foo
[          clap::output::help] 	Help::val: Has switch...
[          clap::output::help] 	Yes
[          clap::output::help] 	Help::val: force_next_line...false
[          clap::output::help] 	Help::val: nlh...false
[          clap::output::help] 	Help::val: taken...21
[          clap::output::help] 	val: help_width > (width - taken)...0 > (100 - 21)
[          clap::output::help] 	Help::val: longest...9
[          clap::output::help] 	Help::val: next_line...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_nspaces!: num=8
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_arg
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=help
[          clap::output::help] 	Help::spec_vals: a=--help
[          clap::output::help] 	Help::val: Has switch...
[          clap::output::help] 	Yes
[          clap::output::help] 	Help::val: force_next_line...false
[          clap::output::help] 	Help::val: nlh...false
[          clap::output::help] 	Help::val: taken...21
[          clap::output::help] 	val: help_width > (width - taken)...23 > (100 - 21)
[          clap::output::help] 	Help::val: longest...9
[          clap::output::help] 	Help::val: next_line...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_nspaces!: num=7
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_arg
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=version
[          clap::output::help] 	Help::spec_vals: a=--version
[          clap::output::help] 	Help::val: Has switch...
[          clap::output::help] 	Yes
[          clap::output::help] 	Help::val: force_next_line...false
[          clap::output::help] 	Help::val: nlh...false
[          clap::output::help] 	Help::val: taken...21
[          clap::output::help] 	val: help_width > (width - taken)...26 > (100 - 21)
[          clap::output::help] 	Help::val: longest...9
[          clap::output::help] 	Help::val: next_line...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_nspaces!: num=4
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[           clap::output::fmt] 	is_a_tty: stderr=false
```

---

_Label `T: bug` added by @glittershark on 2021-06-08 13:48_

---

_Label `:money_with_wings: $10` added by @pksunkara on 2021-06-08 13:50_

---

_Label `C: derive macros` added by @pksunkara on 2021-06-08 13:50_

---

_Renamed from "#[clap(flatten)] takes top-level docstring from the flattened struct, not the struct being flattened into" to "`#[clap(flatten)]` takes top-level docstring from the flattened struct, not the struct being flattened into" by @pksunkara on 2021-06-08 13:50_

---

_Comment by @glittershark on 2021-06-08 14:04_

@pksunkara can I take that to mean that this is definitely not intended behavior?

---

_Comment by @pksunkara on 2021-06-08 14:05_

Yup.

---

_Comment by @pickfire on 2021-06-10 11:07_

What if `Flattened` have docs but `Command` does not have docs, it still does not show description?

---

_Referenced in [clap-rs/clap#2531](../../clap-rs/clap/pulls/2531.md) on 2021-06-10 13:57_

---

_Closed by @pksunkara on 2021-08-12 20:34_

---

_Referenced in [clap-rs/clap#2894](../../clap-rs/clap/issues/2894.md) on 2021-10-16 15:03_

---

_Referenced in [TeXitoi/structopt#539](../../TeXitoi/structopt/issues/539.md) on 2024-05-08 09:19_

---

_Referenced in [cloudflare/pingora#235](../../cloudflare/pingora/issues/235.md) on 2024-05-08 09:27_

---
