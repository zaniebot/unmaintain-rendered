---
number: 3606
title: When the root command has a required argument, it is not output to the subcommand help.
type: issue
state: closed
author: mondy
labels:
  - C-bug
  - A-help
  - E-easy
assignees: []
created_at: 2022-04-03T00:16:49Z
updated_at: 2022-04-30T11:41:27Z
url: https://github.com/clap-rs/clap/issues/3606
synced_at: 2026-01-07T13:12:19-06:00
---

# When the root command has a required argument, it is not output to the subcommand help.

---

_Issue opened by @mondy on 2022-04-03 00:16_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.59.0 (9d1b2106e 2022-02-23)

### Clap Version

clap = { version = "3.1.7", features = ["derive"] }

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Parser)]
#[clap(author, version)]
struct Cli {
    name: String,

    #[clap(subcommand)]
    command: Option<Commands>,
}

#[derive(Subcommand)]
enum Commands {
    Test {},
}

fn main() {
    let cli = Cli::parse();

    println!("Value for name: {}", cli.name);
}

```


### Steps to reproduce the bug with the above code

Execute `cargo run help test`.

### Actual Behaviour

The following is output.
```
excmd.exe-test

USAGE:
    excmd.exe test

OPTIONS:
    -h, --help    Print help information
```

### Expected Behaviour

```
excmd.exe-test

USAGE:
    excmd.exe <NAME> test

OPTIONS:
    -h, --help    Print help information
```

### Additional Context

If you try to execute `cargo run test`, it requires the NAME argument as follows.

```
error: The following required arguments were not provided:
    <NAME>

USAGE:
    excmd.exe <NAME> [SUBCOMMAND]

For more information try --help
error: process didn't exit successfully: `target\debug\excmd.exe test` (exit code: 2)
```

Execute `cargo run test --help` produces the expected output.

```
excmd.exe-test

USAGE:
    excmd.exe <NAME> test

OPTIONS:
    -h, --help    Print help information
```

As an additional preference, it would be easier to understand if it could be executed in a sequence such as `excmd.exe test <NAME>`.

Also, it would be easier to understand if the subcommand takes over only when specified as follows.

```rust
struct Cli {
    #[clap(global = true)]
    name: String,
}
```

### Debug Output

Execute `cargo run help test`.

```
[        clap::build::command]  App::_do_parse
[        clap::build::command]  App::_build
[        clap::build::command]  App::_propagate:excmd
[        clap::build::command]  App::_check_help_and_version: excmd
[        clap::build::command]  App::_check_help_and_version: Building help subcommand
[        clap::build::command]  App::_propagate_global_args:excmd
[        clap::build::command]  App::_derive_display_order:excmd
[        clap::build::command]  App::_derive_display_order:test
[        clap::build::command]  App::_derive_display_order:help
[  clap::build::debug_asserts]  Command::_debug_asserts
[  clap::build::debug_asserts]  Arg::_debug_asserts:help
[  clap::build::debug_asserts]  Arg::_debug_asserts:version
[  clap::build::debug_asserts]  Arg::_debug_asserts:name
[  clap::build::debug_asserts]  Command::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsString("help")' ([104, 101, 108, 112])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=RawOsStr("help")
[         clap::parse::parser]  Parser::get_matches_with: sc=Some("help")
[         clap::parse::parser]  Parser::parse_help_subcommand
[        clap::build::command]  App::_build
[        clap::build::command]  App::_propagate:test
[        clap::build::command]  App::_check_help_and_version: test
[        clap::build::command]  App::_check_help_and_version: Removing generated version
[        clap::build::command]  App::_propagate_global_args:test
[        clap::build::command]  App::_derive_display_order:test
[  clap::build::debug_asserts]  Command::_debug_asserts
[  clap::build::debug_asserts]  Arg::_debug_asserts:help
[  clap::build::debug_asserts]  Command::_verify_positionals
[         clap::parse::parser]  Parser::use_long_help
[         clap::parse::parser]  Parser::help_err: use_long=false
[         clap::parse::parser]  Parser::use_long_help
[        clap::build::command]  App::color: Color setting...
[        clap::build::command]  Auto
[          clap::output::help]  Help::new
[          clap::output::help]  Help::write_help
[          clap::output::help]  should_show_arg: use_long=false, arg=help
[          clap::output::help]  Help::write_templated_help
[          clap::output::help]  Help::write_before_help
[          clap::output::help]  Help::write_bin_name
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage]  Usage::needs_options_tag
[         clap::output::usage]  Usage::needs_options_tag:iter: f=help
[         clap::output::usage]  Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage]  Usage::needs_options_tag: [OPTIONS] not required
[         clap::output::usage]  Usage::create_help_usage: usage=excmd.exe test
[          clap::output::help]  Help::write_all_args
[          clap::output::help]  should_show_arg: use_long=false, arg=help
[          clap::output::help]  Help::write_args
[          clap::output::help]  should_show_arg: use_long=false, arg=help
[          clap::output::help]  Help::write_args: Current Longest...2
[          clap::output::help]  Help::write_args: New Longest...6
[          clap::output::help]  should_show_arg: use_long=false, arg=help
[          clap::output::help]  Help::spec_vals: a=--help
[          clap::output::help]  Help::spec_vals: a=--help
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=help
[          clap::output::help]  Help::align_to_about: arg=help
[          clap::output::help]  Help::align_to_about: Has switch...
[          clap::output::help]  Yes
[          clap::output::help]  Help::align_to_about: nlh...false
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No
[          clap::output::help]  Help::write_after_help
[        clap::build::command]  App::color: Color setting...
[        clap::build::command]  Auto
excmd.exe-test

USAGE:
    excmd.exe test

OPTIONS:
    -h, --help    Print help information
```

---

_Label `C-bug` added by @mondy on 2022-04-03 00:16_

---

_Comment by @epage on 2022-04-03 00:58_

For the specific issue covered in here, what we are doing is not correct but its also a low priority to me for addressing (and the fix just maybe not be worth the complexity) because I believe its generally expected that top-level args are either exclusive with the subcommand or  are global.  For global required, we have https://github.com/clap-rs/clap/issues/1546.

---

_Label `A-help` added by @epage on 2022-04-03 00:58_

---

_Label `E-medium` added by @epage on 2022-04-03 00:58_

---

_Comment by @epage on 2022-04-29 15:53_

I missed that this does work with `sub --help` but not `help sub`.  We have the code for this feature, we just are inconsistently using it.  This should be rather trivial to fix.

---

_Label `E-medium` removed by @epage on 2022-04-29 15:54_

---

_Label `E-easy` added by @epage on 2022-04-29 15:54_

---

_Added to milestone `3.x` by @epage on 2022-04-29 15:54_

---

_Comment by @epage on 2022-04-30 11:41_

This was fixed in #3667 and should be available in v3.1.13

---

_Closed by @epage on 2022-04-30 11:41_

---
