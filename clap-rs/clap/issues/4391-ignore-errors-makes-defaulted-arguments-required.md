```yaml
number: 4391
title: "`ignore_errors` makes defaulted arguments required when using the derive API"
type: issue
state: open
author: NicholasLYang
labels:
  - C-bug
  - A-parsing
  - E-medium
  - A-derive
assignees: []
created_at: 2022-10-14T20:48:19Z
updated_at: 2025-05-07T14:22:40Z
url: https://github.com/clap-rs/clap/issues/4391
synced_at: 2026-01-12T16:14:15Z
```

# `ignore_errors` makes defaulted arguments required when using the derive API

---

_@NicholasLYang_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.66.0-nightly (e7119a030 2022-09-22)

### Clap Version

4.0.15

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser, Debug)]
#[clap(author, version, ignore_errors = true)]
struct Args {
    /// Force color usage in the terminal
    #[clap(long, global = true)]
    color: bool,
}

fn main() {
    let args = Args::parse();
    println!("{}", args.color)
}
```


### Steps to reproduce the bug with the above code

`cargo run -- --help`

### Actual Behaviour

> error: The following required argument was not provided: color
>
>Usage: clap-test [OPTIONS]
>
>For more information try '--help'

### Expected Behaviour

>false


### Additional Context

The expected behavior occurs in version 3.2.22. 

### Debug Output

> [      clap::builder::command] 	Command::_do_parse
> [      clap::builder::command] 	Command::_build: name="clap-test"
> [      clap::builder::command] 	Command::_propagate:clap-test
> [      clap::builder::command] 	Command::_check_help_and_version:clap-test expand_help_tree=false
> [      clap::builder::command] 	Command::long_help_exists
> [      clap::builder::command] 	Command::_check_help_and_version: Building default --help
> [      clap::builder::command] 	Command::_check_help_and_version: Building default --version
> [      clap::builder::command] 	Command::_propagate_global_args:clap-test
> [clap::builder::debug_asserts] 	Command::_debug_asserts
> [clap::builder::debug_asserts] 	Arg::_debug_asserts:color
> [clap::builder::debug_asserts] 	Arg::_debug_asserts:help
> [clap::builder::debug_asserts] 	Arg::_debug_asserts:version
> [clap::builder::debug_asserts] 	Command::_verify_positionals
> [        clap::parser::parser] 	Parser::get_matches_with
> [        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--help")' ([45, 45, 104, 101, 108, 112])
> [        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("--help")
> [        clap::parser::parser] 	Parser::get_matches_with: sc=None
> [        clap::parser::parser] 	Parser::parse_long_arg
> [        clap::parser::parser] 	Parser::parse_long_arg: Does it contain '='...
> [        clap::parser::parser] 	Parser::parse_long_arg: Found valid arg or flag '--help'
> [        clap::parser::parser] 	Parser::parse_long_arg("help"): Presence validated
> [        clap::parser::parser] 	Parser::react action=Help, identifier=Some(Long), source=CommandLine
> [        clap::parser::parser] 	Help: use_long=true
> [      clap::builder::command] 	Command::long_help_exists: false
> [      clap::builder::command] 	Command::write_help_err: clap-test, use_long=false
> [      clap::builder::command] 	Command::long_help_exists: false
> [          clap::output::help] 	write_help
> [ clap::output::help_template] 	HelpTemplate::new cmd=clap-test, use_long=false
> [ clap::output::help_template] 	should_show_arg: use_long=false, arg=color
> [ clap::output::help_template] 	HelpTemplate::write_templated_help
> [ clap::output::help_template] 	HelpTemplate::write_before_help
> [         clap::output::usage] 	Usage::create_usage_no_title
> [         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
> [         clap::output::usage] 	Usage::needs_options_tag
> [         clap::output::usage] 	Usage::needs_options_tag:iter: f=color
> [      clap::builder::command] 	Command::groups_for_arg: id="color"
> [         clap::output::usage] 	Usage::needs_options_tag:iter:iter: grp_s="Args"
> [         clap::output::usage] 	Usage::needs_options_tag:iter: [OPTIONS] required
> [         clap::output::usage] 	Usage::get_args: incls=[]
> [         clap::output::usage] 	Usage::get_args: unrolled_reqs=[]
> [         clap::output::usage] 	Usage::get_args: ret_val=[]
> [         clap::output::usage] 	Usage::create_help_usage: usage=clap-test [OPTIONS]
> [ clap::output::help_template] 	HelpTemplate::write_all_args
> [ clap::output::help_template] 	should_show_arg: use_long=false, arg=color
> [ clap::output::help_template] 	should_show_arg: use_long=false, arg=help
> [ clap::output::help_template] 	should_show_arg: use_long=false, arg=version
> [ clap::output::help_template] 	HelpTemplate::write_args Options
> [ clap::output::help_template] 	should_show_arg: use_long=false, arg=color
> [ clap::output::help_template] 	HelpTemplate::write_args: arg="color" longest=7
> [ clap::output::help_template] 	should_show_arg: use_long=false, arg=help
> [ clap::output::help_template] 	HelpTemplate::write_args: arg="help" longest=7
> [ clap::output::help_template] 	should_show_arg: use_long=false, arg=version
> [ clap::output::help_template] 	HelpTemplate::write_args: arg="version" longest=9
> [ clap::output::help_template] 	should_show_arg: use_long=false, arg=color
> [ clap::output::help_template] 	HelpTemplate::spec_vals: a=--color
> [ clap::output::help_template] 	should_show_arg: use_long=false, arg=help
> [ clap::output::help_template] 	HelpTemplate::spec_vals: a=--help
> [ clap::output::help_template] 	should_show_arg: use_long=false, arg=version
> [ clap::output::help_template] 	HelpTemplate::spec_vals: a=--version
> [ clap::output::help_template] 	HelpTemplate::spec_vals: a=--color
> [ clap::output::help_template] 	HelpTemplate::short
> [ clap::output::help_template] 	HelpTemplate::long
> [ clap::output::help_template] 	HelpTemplate::align_to_about: arg=color, next_line_help=false, longest=9
> [ clap::output::help_template] 	HelpTemplate::align_to_about: positional=false arg_len=7, spaces=4
> [ clap::output::help_template] 	HelpTemplate::help
> [ clap::output::help_template] 	HelpTemplate::help: help_width=17, spaces=33, avail=83
> [ clap::output::help_template] 	HelpTemplate::spec_vals: a=--help
> [ clap::output::help_template] 	HelpTemplate::short
> [ clap::output::help_template] 	HelpTemplate::long
> [ clap::output::help_template] 	HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=9
> [ clap::output::help_template] 	HelpTemplate::align_to_about: positional=false arg_len=6, spaces=5
> [ clap::output::help_template] 	HelpTemplate::help
> [ clap::output::help_template] 	HelpTemplate::help: help_width=17, spaces=22, avail=83
> [ clap::output::help_template] 	HelpTemplate::spec_vals: a=--version
> [ clap::output::help_template] 	HelpTemplate::short
> [ clap::output::help_template] 	HelpTemplate::long
> [ clap::output::help_template] 	HelpTemplate::align_to_about: arg=version, next_line_help=false, longest=9
> [ clap::output::help_template] 	HelpTemplate::align_to_about: positional=false arg_len=9, spaces=2
> [ clap::output::help_template] 	HelpTemplate::help
> [ clap::output::help_template] 	HelpTemplate::help: help_width=17, spaces=25, avail=83
> [ clap::output::help_template] 	HelpTemplate::write_after_help
> [      clap::builder::command] 	Command::color: Color setting...
> [      clap::builder::command] 	Auto
> [      clap::builder::command] 	Command::color: Color setting...
> [      clap::builder::command] 	Auto
> [      clap::builder::command] 	Command::_do_parse: ignoring error: Usage: clap-test [OPTIONS]
> 
> Options:
>       --color    Force color usage in the terminal
>   -h, --help     Print help information
>   -V, --version  Print version information
> 
> Backtrace:
>    0: backtrace::backtrace::libunwind::trace
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.66/src/backtrace/libunwind.rs:93:5
>       backtrace::backtrace::trace_unsynchronized
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.66/src/backtrace/mod.rs:66:5
>    1: backtrace::backtrace::trace
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.66/src/backtrace/mod.rs:53:14
>    2: backtrace::capture::Backtrace::create
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.66/src/capture.rs:176:9
>    3: backtrace::capture::Backtrace::new
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.66/src/capture.rs:140:22
>    4: clap::error::Backtrace::new
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/error/mod.rs:835:19
>    5: clap::error::Error<F>::new
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/error/mod.rs:135:28
>    6: clap::error::Error<F>::for_app
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/error/mod.rs:277:9
>    7: clap::error::Error<F>::display_help
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/error/mod.rs:329:9
>    8: clap::parser::parser::Parser::help_err
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/parser/parser.rs:1584:9
>    9: clap::parser::parser::Parser::react
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/parser/parser.rs:1288:21
>   10: clap::parser::parser::Parser::parse_long_arg
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/parser/parser.rs:815:17
>   11: clap::parser::parser::Parser::get_matches_with
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/parser/parser.rs:132:44
>   12: clap::builder::command::Command::_do_parse
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/builder/command.rs:3794:29
>   13: clap::builder::command::Command::try_get_matches_from_mut
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/builder/command.rs:708:9
>   14: clap::builder::command::Command::get_matches_from
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/builder/command.rs:578:9
>   15: clap::builder::command::Command::get_matches
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/builder/command.rs:490:9
>   16: clap::derive::Parser::parse
>              at /Users/nicholas/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.15/src/derive.rs:82:27
>   17: clap_test::main
>              at src/main.rs:12:16
>   18: core::ops::function::FnOnce::call_once
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/core/src/ops/function.rs:248:5
>   19: std::sys_common::backtrace::__rust_begin_short_backtrace
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/sys_common/backtrace.rs:122:18
>   20: std::rt::lang_start::{{closure}}
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/rt.rs:145:18
>   21: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/core/src/ops/function.rs:280:13
>       std::panicking::try::do_call
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/panicking.rs:492:40
>       std::panicking::try
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/panicking.rs:456:19
>       std::panic::catch_unwind
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/panic.rs:137:14
>       std::rt::lang_start_internal::{{closure}}
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/rt.rs:128:48
>       std::panicking::try::do_call
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/panicking.rs:492:40
>       std::panicking::try
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/panicking.rs:456:19
>       std::panic::catch_unwind
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/panic.rs:137:14
>       std::rt::lang_start_internal
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/rt.rs:128:20
>   22: std::rt::lang_start
>              at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/rt.rs:144:17
>   23: _main
> 
> 
> [   clap::parser::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=["color"]
> [      clap::builder::command] 	Command::_build: name="clap-test"
> [      clap::builder::command] 	Command::_propagate:clap-test
> [      clap::builder::command] 	Command::_check_help_and_version:clap-test expand_help_tree=false
> [      clap::builder::command] 	Command::long_help_exists
> [      clap::builder::command] 	Command::_check_help_and_version: Building default --help
> [      clap::builder::command] 	Command::_check_help_and_version: Building default --version
> [      clap::builder::command] 	Command::_propagate_global_args:clap-test
> [clap::builder::debug_asserts] 	Command::_debug_asserts
> [clap::builder::debug_asserts] 	Arg::_debug_asserts:color
> [clap::builder::debug_asserts] 	Arg::_debug_asserts:help
> [clap::builder::debug_asserts] 	Arg::_debug_asserts:version
> [clap::builder::debug_asserts] 	Command::_verify_positionals
> [      clap::builder::command] 	Command::_build: name="clap-test"
> [      clap::builder::command] 	Command::_build: already built
> [         clap::output::usage] 	Usage::create_usage_with_title
> [         clap::output::usage] 	Usage::create_usage_no_title
> [         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
> [         clap::output::usage] 	Usage::needs_options_tag
> [         clap::output::usage] 	Usage::needs_options_tag:iter: f=color
> [      clap::builder::command] 	Command::groups_for_arg: id="color"
> [         clap::output::usage] 	Usage::needs_options_tag:iter:iter: grp_s="Args"
> [         clap::output::usage] 	Usage::needs_options_tag:iter: [OPTIONS] required
> [         clap::output::usage] 	Usage::get_args: incls=[]
> [         clap::output::usage] 	Usage::get_args: unrolled_reqs=[]
> [         clap::output::usage] 	Usage::get_args: ret_val=[]
> [         clap::output::usage] 	Usage::create_help_usage: usage=clap-test [OPTIONS]
> [      clap::builder::command] 	Command::color: Color setting...
> [      clap::builder::command] 	Auto
> [      clap::builder::command] 	Command::color: Color setting...
> [      clap::builder::command] 	Auto
> error: The following required argument was not provided: color
> 
> Usage: clap-test [OPTIONS]
> 
> For more information try '--help'
> 

---

_Label `C-bug` added by @NicholasLYang on 2022-10-14 20:48_

---

_Comment by @epage on 2022-10-14 21:20_

You can work around this by switching to an `Option<bool>` but that will likely require also adding `#[arg(action = ArgAction::SetTrue)]`.

I'm assuming you can reproduce this in clap v3 if you add `#[clap(action)]`.  When we switched flags from `ArgAction::IncOccurrences` to `ArgAction::SetTrue`, we switched from just tracking of we had encountered the flag on the command line to inserting a `true`, defaulting to `false` This makes flags behave like everything else, making things more consistent / easier to use.

The workaround works because the error is coming from `clap_derive` when it is trying to fill in `color`, it has no value to put there so it errors.

My guess is that in at least some cases of ignore_errors, we are not processing defaults.  This means the problem would be the same if you did `#[arg(long, default_value = "-")] input: PathBuf`

We should probably be making sure we populate defaults with `ignore_errors`.  With the derive API, this turns a broken use case to a working one, so no compatibility concerns.  With the builder API, it is a little more complicated.

---

_Label `A-parsing` added by @epage on 2022-10-14 21:20_

---

_Label `E-medium` added by @epage on 2022-11-08 04:28_

---

_Referenced in [clap-rs/clap#4498](../../clap-rs/clap/issues/4498.md) on 2022-11-21 15:39_

---

_Renamed from "`ignore_errors` makes boolean flag required in clap 4" to "`ignore_errors` makes defaulted arguments required when using the derive API" by @epage on 2025-05-07 14:21_

---

_Label `A-derive` added by @epage on 2025-05-07 14:22_

---
