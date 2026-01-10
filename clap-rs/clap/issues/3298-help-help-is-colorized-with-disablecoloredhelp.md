---
number: 3298
title: "`help help` is colorized with DisableColoredHelp"
type: issue
state: closed
author: ehuss
labels:
  - C-bug
assignees: []
created_at: 2022-01-16T19:29:16Z
updated_at: 2022-01-17T15:38:13Z
url: https://github.com/clap-rs/clap/issues/3298
synced_at: 2026-01-10T01:27:38Z
---

# `help help` is colorized with DisableColoredHelp

---

_Issue opened by @ehuss on 2022-01-16 19:29_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.60.0-nightly (ec4bcaac4 2022-01-15)

### Clap Version

3.0.7

### Minimal reproducible code

```rust
use clap::{App, AppSettings};

fn main() {
    let _matches = App::new("myapp")
        .global_setting(AppSettings::DisableColoredHelp)
        .subcommand(App::new("foo"))
        .get_matches();
}
```


### Steps to reproduce the bug with the above code

`cargo run -- help help`

### Actual Behaviour

The help output is colored.

<img width="487" alt="image" src="https://user-images.githubusercontent.com/43198/149674792-4f7fcb74-fe34-4aa5-906a-c51a690b6fe0.png">

versus other help output:

<img width="443" alt="image" src="https://user-images.githubusercontent.com/43198/149674811-6bc3b86b-0f58-4dce-b47f-4ae30b16777f.png">


### Expected Behaviour

The help output should not be colored due to `DisableColoredHelp`.

### Additional Context

The priority of this is the lowest of the low.  Just noticed it and thought I'd report it.


### Debug Output

```
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:myapp
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Removing generated version
[            clap::build::app] 	App::_check_help_and_version: Building help subcommand
[            clap::build::app] 	App::_propagate_global_args:myapp
[            clap::build::app] 	App::_derive_display_order:myapp
[            clap::build::app] 	App::_derive_display_order:foo
[            clap::build::app] 	App::_derive_display_order:help
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::app::debug_asserts] 	App::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("help")' ([104, 101, 108, 112])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("help")
[         clap::parse::parser] 	Parser::get_matches_with: sc=Some("help")
[         clap::parse::parser] 	Parser::parse_help_subcommand
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:help
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Removing generated help
[            clap::build::app] 	App::_check_help_and_version: Removing generated version
[            clap::build::app] 	App::_propagate_global_args:help
[            clap::build::app] 	App::_derive_display_order:help
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:subcommand
[clap::build::app::debug_asserts] 	App::_verify_positionals
[         clap::parse::parser] 	Parser::help_err: use_long=false
[            clap::build::app] 	App::color: Color setting...
[            clap::build::app] 	Auto
[          clap::output::help] 	Help::new
[          clap::output::help] 	Help::write_help
[          clap::output::help] 	should_show_arg: use_long=false, arg=subcommand
[          clap::output::help] 	Help::write_templated_help
[          clap::output::help] 	Help::write_before_help
[          clap::output::help] 	Help::write_bin_name
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage] 	Usage::needs_options_tag
[         clap::output::usage] 	Usage::needs_options_tag: [OPTIONS] not required
[         clap::output::usage] 	Usage::get_args_tag; incl_reqs = true
[         clap::output::usage] 	Usage::get_args_tag:iter:subcommand
[            clap::build::app] 	App::groups_for_arg: id=subcommand
[         clap::output::usage] 	Usage::get_args_tag:iter: 1 Args not required or hidden
[            clap::build::app] 	App::groups_for_arg: id=subcommand
[         clap::output::usage] 	Usage::get_args_tag:iter: Exactly one, returning 'subcommand'
[            clap::build::arg] 	Arg::name_no_brackets:subcommand
[            clap::build::arg] 	Arg::name_no_brackets: val_names=[
    "SUBCOMMAND",
]
[         clap::output::usage] 	Usage::create_help_usage: usage=foo help [SUBCOMMAND]...
[          clap::output::help] 	Help::write_all_args
[          clap::output::help] 	should_show_arg: use_long=false, arg=subcommand
[          clap::output::help] 	Help::write_args_unsorted
[          clap::output::help] 	should_show_arg: use_long=false, arg=subcommand
[          clap::output::help] 	should_show_arg: use_long=false, arg=subcommand
[          clap::output::help] 	Help::spec_vals: a=<SUBCOMMAND>...
[          clap::output::help] 	Help::spec_vals: a=<SUBCOMMAND>...
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=subcommand
[          clap::output::help] 	Help::val: Has switch...
[          clap::output::help] 	No, and not next_line
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_after_help
foo-help
Print this message or the help of the given subcommand(s)

USAGE:
    foo help [SUBCOMMAND]...

ARGS:
    <SUBCOMMAND>...    The subcommand whose help message to display
```

---

_Label `C-bug` added by @ehuss on 2022-01-16 19:29_

---

_Comment by @epage on 2022-01-17 14:22_

Looks like we propagate the global settings before creating the `help` subcommand`.  The main question I have is whether its safe to swap the order (if the help and version logic needs some of the work being done in the previous step).  I'll probably make a more immediate change of propagating settings just for the help subcommand.

---

_Referenced in [clap-rs/clap#3305](../../clap-rs/clap/pulls/3305.md) on 2022-01-17 15:23_

---

_Closed by @epage on 2022-01-17 15:35_

---

_Comment by @epage on 2022-01-17 15:38_

v3.0.8 is now released with the fix for this

---
