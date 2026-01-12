```yaml
number: 2454
title: Required trailing var arg positional should be printed last in usage
type: issue
state: closed
author: jarrodldavis
labels:
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-04-25T23:51:33Z
updated_at: 2021-05-20T18:59:22Z
url: https://github.com/clap-rs/clap/issues/2454
synced_at: 2026-01-12T16:14:13Z
```

# Required trailing var arg positional should be printed last in usage

---

_@jarrodldavis_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

`rustc 1.51.0 (2fd73fabe 2021-03-23)`

### Affected Version of clap

Latest `master` ([`d253c34676acf73c7ad04bcec56ff573e925681d`](https://github.com/clap-rs/clap/commit/d253c34676acf73c7ad04bcec56ff573e925681d))

### Expected Behavior Summary
A required trailing var arg positional should always be printed last for a command's usage, since the `AppSettings::TrailingVarArg` parsing behavior only works correctly when the positional argument is indeed last, after any other required or optional flags, options, and positionals.

### Actual Behavior Summary
A required trailing var arg positional is printed first, since `clap` reorders arguments in the usage line to print required positionals first, followed by required options, then optional arguments.

### Steps to Reproduce the issue
```rust
use std::ffi::OsString;
use std::path::PathBuf;
use clap::{Clap, AppSettings, ValueHint};

/// Run a program.
#[derive(Clap, Debug)]
#[clap(setting = AppSettings::TrailingVarArg)]
struct Opt {
    /// Record stdout to <file>
    #[clap(short, long, parse(from_os_str), value_hint = ValueHint::FilePath, value_name = "file")]
    outfile: PathBuf,

    /// Record stderr to <file>
    #[clap(short, long, parse(from_os_str), value_hint = ValueHint::FilePath, value_name = "file")]
    errfile: PathBuf,

    /// The command to run, with its arguments
    #[clap(required = true, parse(from_os_str), value_hint = ValueHint::CommandWithArguments, name = "cmd-with-args")]
    cmd: Vec<OsString>,
}

fn main() {
    let opt = Opt::parse();
    println!("{:?}", opt);
}
```

### Sample Code or Link to Sample Code
https://github.com/jarrodldavis/clap/blob/trailing-var-arg-order-issue/clap_derive/examples/trailing_var_arg.rs

### Debug output

<details>
<summary> Debug Output </summary>

```
$ cargo run --example trailing_var_arg -- --help    
    Finished dev [unoptimized + debuginfo] target(s) in 0.06s
     Running `target/debug/examples/trailing_var_arg --help`
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:clap_derive
[            clap::build::app]  App::_check_help_and_version
[            clap::build::app]  App::_propagate_global_args:clap_derive
[            clap::build::app]  App::_derive_display_order:clap_derive
[clap::build::app::debug_asserts]       App::_debug_asserts
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:help
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:version
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:outfile
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:errfile
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:cmd-with-args
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing '"--help"' ([45, 45, 104, 101, 108, 112])
[         clap::parse::parser]  Parser::possible_subcommand: arg="--help"
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::is_new_arg: "--help":NotFound
[         clap::parse::parser]  Parser::is_new_arg: want_value=false
[         clap::parse::parser]  Parser::is_new_arg: -- found
[         clap::parse::parser]  Parser::parse_long_arg
[         clap::parse::parser]  Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser]  No
[         clap::parse::parser]  Parser::parse_long_arg: Found valid opt or flag '--help'
[         clap::parse::parser]  Parser::check_for_help_and_version_str
[         clap::parse::parser]  Parser::check_for_help_and_version_str: Checking if --"help" is help or version...
[         clap::parse::parser]  Help
[         clap::parse::parser]  [         clap::parse::parser]  Parser::use_long_help
Parser::help_err: use_long=false
[         clap::parse::parser]  Parser::use_long_help
[         clap::parse::parser]  Parser::color_help
[          clap::output::help]  Help::new
[          clap::output::help]  Help::write_help
[          clap::output::help]  should_show_arg: use_long=false, arg=cmd-with-args
[          clap::output::help]  should_show_arg: use_long=false, arg=help
[          clap::output::help]  should_show_arg: use_long=false, arg=outfile
[          clap::output::help]  Help::write_templated_help
[          clap::output::help]  Help::write_before_help
[          clap::output::help]  Help::write_bin_name
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs=[outfile, errfile, cmd-with-args]
[         clap::output::usage]  Usage::get_required_usage_from:iter:cmd-with-args
[         clap::output::usage]  Usage::get_required_usage_from:iter:outfile
[         clap::output::usage]  Usage::get_required_usage_from:iter:errfile
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=["<cmd-with-args>...", "--outfile <file>", "--errfile <file>"]
[         clap::output::usage]  Usage::needs_flags_tag
[         clap::output::usage]  Usage::needs_flags_tag:iter: f=help
[         clap::output::usage]  Usage::needs_flags_tag:iter: f=version
[         clap::output::usage]  Usage::needs_flags_tag: [FLAGS] not required
[         clap::output::usage]  Usage::create_help_usage: usage=trailing_var_arg <cmd-with-args>... --outfile <file> --errfile <file>
[          clap::output::help]  Help::write_all_args
[          clap::output::help]  should_show_arg: use_long=false, arg=cmd-with-args
[          clap::output::help]  should_show_arg: use_long=false, arg=help
[          clap::output::help]  should_show_arg: use_long=false, arg=version
[          clap::output::help]  should_show_arg: use_long=false, arg=outfile
[          clap::output::help]  should_show_arg: use_long=false, arg=errfile
[          clap::output::help]  Help::write_args_unsorted
[          clap::output::help]  should_show_arg: use_long=false, arg=cmd-with-args
[          clap::output::help]  should_show_arg: use_long=false, arg=cmd-with-args
[          clap::output::help]  Help::spec_vals: a=<cmd-with-args>...
[          clap::output::help]  Help::spec_vals: a=<cmd-with-args>...
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=cmd-with-args
[          clap::output::help]  Help::val: Has switch...
[          clap::output::help]  No, and not next_line
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No
[          clap::output::help]  Help::write_args
[          clap::output::help]  should_show_arg: use_long=false, arg=help
[          clap::output::help]  Help::write_args: Current Longest...2
[          clap::output::help]  Help::write_args: New Longest...6
[          clap::output::help]  should_show_arg: use_long=false, arg=version
[          clap::output::help]  Help::write_args: Current Longest...6
[          clap::output::help]  Help::write_args: New Longest...9
[          clap::output::help]  should_show_arg: use_long=false, arg=help
[          clap::output::help]  Help::spec_vals: a=--help
[          clap::output::help]  should_show_arg: use_long=false, arg=version
[          clap::output::help]  Help::spec_vals: a=--version
[          clap::output::help]  Help::spec_vals: a=--help
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=help
[          clap::output::help]  Help::val: Has switch...
[          clap::output::help]  Yes
[          clap::output::help]  Help::val: nlh...false
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No
[          clap::output::help]  Help::spec_vals: a=--version
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=version
[          clap::output::help]  Help::val: Has switch...
[          clap::output::help]  Yes
[          clap::output::help]  Help::val: nlh...false
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No
[          clap::output::help]  Help::write_args
[          clap::output::help]  should_show_arg: use_long=false, arg=outfile
[          clap::output::help]  Help::write_args: Current Longest...2
[          clap::output::help]  Help::write_args: New Longest...16
[          clap::output::help]  should_show_arg: use_long=false, arg=errfile
[          clap::output::help]  Help::write_args: Current Longest...16
[          clap::output::help]  Help::write_args: New Longest...16
[          clap::output::help]  should_show_arg: use_long=false, arg=outfile
[          clap::output::help]  Help::spec_vals: a=--outfile <file>
[          clap::output::help]  should_show_arg: use_long=false, arg=errfile
[          clap::output::help]  Help::spec_vals: a=--errfile <file>
[          clap::output::help]  Help::spec_vals: a=--errfile <file>
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=errfile
[          clap::output::help]  Help::val: Has switch...
[          clap::output::help]  Yes
[          clap::output::help]  Help::val: nlh...false
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No
[          clap::output::help]  Help::spec_vals: a=--outfile <file>
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=outfile
[          clap::output::help]  Help::val: Has switch...
[          clap::output::help]  Yes
[          clap::output::help]  Help::val: nlh...false
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No
[          clap::output::help]  Help::write_after_help
[           clap::output::fmt]  is_a_tty: stderr=false
clap_derive 

Run a program

USAGE:
    trailing_var_arg <cmd-with-args>... --outfile <file> --errfile <file>

ARGS:
    <cmd-with-args>...    The command to run, with its arguments

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
    -e, --errfile <file>    Record stderr to <file>
    -o, --outfile <file>    Record stdout to <file>
```

</details>


---

_Label `C: usage strings` added by @pksunkara on 2021-05-18 18:26_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-05-18 18:26_

---

_Referenced in [clap-rs/clap#2492](../../clap-rs/clap/pulls/2492.md) on 2021-05-20 15:47_

---

_Closed by @pksunkara on 2021-05-20 18:59_

---
