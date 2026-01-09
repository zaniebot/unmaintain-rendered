---
number: 3572
title: "`arg_required_else_help` does not show help when env variables are present"
type: issue
state: open
author: willbuckner
labels:
  - C-bug
  - A-help
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2022-03-24T11:57:59Z
updated_at: 2024-11-04T15:39:07Z
url: https://github.com/clap-rs/clap/issues/3572
synced_at: 2026-01-07T13:12:19-06:00
---

# `arg_required_else_help` does not show help when env variables are present

---

_Issue opened by @willbuckner on 2022-03-24 11:57_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.58.1 (db9d1b20b 2022-01-20)

### Clap Version

clap = { version = "3.1.6", features = ["env", "std", "cargo"] }

### Minimal reproducible code

### Cargo.toml
```toml
[package]
name = "claptest"
version = "0.1.0"
edition = "2021"

[[bin]]
name = "broken"
path = "src/broken.rs"

[[bin]]
name = "working"
path = "src/working.rs"

[[bin]]
name = "deprecated-working"
path = "src/deprecated-working.rs"

[dependencies]
clap = { version = "3.1.6", features = ["env", "std", "cargo"] }
```

### src/broken.rs
```rust
use clap::{Arg, Command};
use std::env;

fn main() {
    env::set_var("CLAPTEST", "test");
    println!("CLAPTEST: {}", env::var("CLAPTEST").unwrap());

    println!("Broken:");
    let cmd = Command::new("test")
        .subcommand_required(true)
        .arg_required_else_help(true)
        .arg(
            Arg::new("WUT2")
                .short('W')
                .takes_value(true)
                .env("CLAPTEST"),
        )
        .subcommand(Command::new("sub").arg(Arg::new("SUBARG").short('s')));

    let _args = cmd.get_matches();
}
```

### src/working.rs
```rust
use clap::{Arg, Command};
use std::env;

fn main() {
    env::set_var("CLAPTEST", "test");
    println!("CLAPTEST: {}", env::var("CLAPTEST").unwrap());

    println!("Working:");
    let cmd = Command::new("test")
        .subcommand_required(true)
        .arg_required_else_help(true)
        .arg(
            Arg::new("WUT2")
                .short('W')
                .takes_value(true)
        )
        .subcommand(Command::new("sub").arg(Arg::new("SUBARG").short('s')));

    let _args = cmd.get_matches();
}
```

### src/deprecated-working.rs
```rust
use clap::{AppSettings, Arg, Command};
use std::env;

fn main() {
    env::set_var("CLAPTEST", "test");
    println!("CLAPTEST: {}", env::var("CLAPTEST").unwrap());

    println!("Deprecated:");
    #[allow(deprecated)]
    let cmd = Command::new("test")
        .setting(AppSettings::SubcommandRequiredElseHelp)
        .arg(
            Arg::new("WUT2")
                .short('W')
                .takes_value(true)
                .env("CLAPTEST"),
        )
        .subcommand(Command::new("sub").arg(Arg::new("SUBARG").short('s')));

    let _args = cmd.get_matches();
}
```


### Steps to reproduce the bug with the above code

### src/deprecated-working.rs
```shell
[*]~/dev/claptest[master #]⦕ cargo run --bin deprecated-working
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/deprecated-working`
CLAPTEST: test
Deprecated:
test

USAGE:
    deprecated-working [OPTIONS] <SUBCOMMAND>

OPTIONS:
    -h, --help       Print help information
    -W <WUT2>        [env: CLAPTEST=test]

SUBCOMMANDS:
    help    Print this message or the help of the given subcommand(s)
    sub
[*]~/dev/claptest[master #]⦕
```

### src/working.rs
```shell
[*]~/dev/claptest[master #]⦕ cargo run --bin working
   Compiling claptest v0.1.0 (/Users/will/dev/claptest)
    Finished dev [unoptimized + debuginfo] target(s) in 1.31s
     Running `target/debug/working`
CLAPTEST: test
Working:
test

USAGE:
    working [OPTIONS] <SUBCOMMAND>

OPTIONS:
    -h, --help       Print help information
    -W <WUT2>

SUBCOMMANDS:
    help    Print this message or the help of the given subcommand(s)
    sub
[*]~/dev/claptest[master #]⦕
```

### src/broken.rs
```shell
[*]~/dev/claptest[master #]⦕ cargo run --bin broken
   Compiling claptest v0.1.0 (/Users/will/dev/claptest)
    Finished dev [unoptimized + debuginfo] target(s) in 1.22s
     Running `target/debug/broken`
CLAPTEST: test
Broken:
error: 'broken' requires a subcommand but one was not provided

USAGE:
    broken [OPTIONS] <SUBCOMMAND>

For more information try --help
[*]~/dev/claptest[master #]⦕
```

### Actual Behaviour

When I use `.env("ENV_VAR")` on an `Arg`, and I have `.subcommand_required(true).arg_required_else_help(true)`, and `ENV_VAR` is set, help is not output, instead a short usage message appears without help.

This is what happens:
### src/broken.rs
```shell
[*]~/dev/claptest[master #]⦕ cargo run --bin broken
   Compiling claptest v0.1.0 (/Users/will/dev/claptest)
    Finished dev [unoptimized + debuginfo] target(s) in 1.22s
     Running `target/debug/broken`
CLAPTEST: test
Broken:
error: 'broken' requires a subcommand but one was not provided

USAGE:
    broken [OPTIONS] <SUBCOMMAND>

For more information try --help
[*]~/dev/claptest[master #]⦕
```

### Expected Behaviour

When I use `.env("ENV_VAR")` on an `Arg`, and I have `.subcommand_required(true).arg_required_else_help(true)`, and `ENV_VAR` is set, **help should be output, instead of a short usage message without help**.

This is what should happen:

### src/deprecated-working.rs
```shell
[*]~/dev/claptest[master #]⦕ cargo run --bin deprecated-working
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/deprecated-working`
CLAPTEST: test
Deprecated:
test

USAGE:
    deprecated-working [OPTIONS] <SUBCOMMAND>

OPTIONS:
    -h, --help       Print help information
    -W <WUT2>        [env: CLAPTEST=test]

SUBCOMMANDS:
    help    Print this message or the help of the given subcommand(s)
    sub
[*]~/dev/claptest[master #]⦕
```

Said another way, **I should be able to emulate the behavior of `AppSettings::SubcommandRequiredElseHelp` without using a deprecated setting.

### Additional Context

Maybe this is a new builder setting; `.subcommand_required_else_help()`, or perhaps `.env_provides_user_arg(false)`, or similar.

Awesome work on Clap btw :)

### Debug Output

### src/broken.rs
```shell
[*]~/dev/claptest[master #]⦕ cargo run --bin broken
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
     Running `target/debug/broken`
CLAPTEST: test
Broken:
[        clap::build::command] 	App::_do_parse
[        clap::build::command] 	App::_build
[        clap::build::command] 	App::_propagate:test
[        clap::build::command] 	App::_check_help_and_version: test
[        clap::build::command] 	App::_check_help_and_version: Removing generated version
[        clap::build::command] 	App::_check_help_and_version: Building help subcommand
[        clap::build::command] 	App::_propagate_global_args:test
[        clap::build::command] 	App::_derive_display_order:test
[        clap::build::command] 	App::_derive_display_order:sub
[        clap::build::command] 	App::_derive_display_order:help
[  clap::build::debug_asserts] 	Command::_debug_asserts
[  clap::build::debug_asserts] 	Arg::_debug_asserts:help
[  clap::build::debug_asserts] 	Arg::_debug_asserts:WUT2
[  clap::build::debug_asserts] 	Command::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_env: Checking arg `--help`
[         clap::parse::parser] 	Parser::add_env: Checking arg `-W <WUT2>`
[         clap::parse::parser] 	Parser::add_env: Found an opt with value=RawOsStr("test"), trailing=false
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=WUT2, val=RawOsStr("test")
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."test"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[        clap::build::command] 	App::groups_for_arg: id=WUT2
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=WUT2
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:WUT2:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:WUT2: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:WUT2: doesn't have default missing vals
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage] 	Usage::needs_options_tag
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=help
[         clap::output::usage] 	Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=WUT2
[        clap::build::command] 	App::groups_for_arg: id=WUT2
[         clap::output::usage] 	Usage::needs_options_tag:iter: [OPTIONS] required
[         clap::output::usage] 	Usage::create_help_usage: usage=broken [OPTIONS] <SUBCOMMAND>
[        clap::build::command] 	App::color: Color setting...
[        clap::build::command] 	Auto
error: 'broken' requires a subcommand but one was not provided

USAGE:
    broken [OPTIONS] <SUBCOMMAND>

For more information try --help
```

### src/working.rs
```shell
[*]~/dev/claptest[master #]⦕ cargo run --bin working
   Compiling claptest v0.1.0 (/Users/will/dev/claptest)
    Finished dev [unoptimized + debuginfo] target(s) in 0.56s
     Running `target/debug/working`
CLAPTEST: test
Working:
[        clap::build::command] 	App::_do_parse
[        clap::build::command] 	App::_build
[        clap::build::command] 	App::_propagate:test
[        clap::build::command] 	App::_check_help_and_version: test
[        clap::build::command] 	App::_check_help_and_version: Removing generated version
[        clap::build::command] 	App::_check_help_and_version: Building help subcommand
[        clap::build::command] 	App::_propagate_global_args:test
[        clap::build::command] 	App::_derive_display_order:test
[        clap::build::command] 	App::_derive_display_order:sub
[        clap::build::command] 	App::_derive_display_order:help
[  clap::build::debug_asserts] 	Command::_debug_asserts
[  clap::build::debug_asserts] 	Arg::_debug_asserts:help
[  clap::build::debug_asserts] 	Arg::_debug_asserts:WUT2
[  clap::build::debug_asserts] 	Command::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_env: Checking arg `--help`
[         clap::parse::parser] 	Parser::add_env: Checking arg `-W <WUT2>`
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:WUT2:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:WUT2: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:WUT2: doesn't have default missing vals
[        clap::build::command] 	App::color: Color setting...
[        clap::build::command] 	Auto
[          clap::output::help] 	Help::new
[          clap::output::help] 	Help::write_help
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::write_templated_help
[          clap::output::help] 	Help::write_before_help
[          clap::output::help] 	Help::write_bin_name
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage] 	Usage::needs_options_tag
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=help
[         clap::output::usage] 	Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=WUT2
[        clap::build::command] 	App::groups_for_arg: id=WUT2
[         clap::output::usage] 	Usage::needs_options_tag:iter: [OPTIONS] required
[         clap::output::usage] 	Usage::create_help_usage: usage=working [OPTIONS] <SUBCOMMAND>
[          clap::output::help] 	Help::write_all_args
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	should_show_arg: use_long=false, arg=WUT2
[          clap::output::help] 	Help::write_args
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::write_args: Current Longest...2
[          clap::output::help] 	Help::write_args: New Longest...6
[          clap::output::help] 	should_show_arg: use_long=false, arg=WUT2
[          clap::output::help] 	Help::write_args: Current Longest...6
[          clap::output::help] 	Help::write_args: New Longest...9
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::spec_vals: a=--help
[          clap::output::help] 	should_show_arg: use_long=false, arg=WUT2
[          clap::output::help] 	Help::spec_vals: a=-W <WUT2>
[          clap::output::help] 	Help::spec_vals: a=--help
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=help
[          clap::output::help] 	Help::align_to_about: arg=help
[          clap::output::help] 	Help::align_to_about: Has switch...
[          clap::output::help] 	Yes
[          clap::output::help] 	Help::align_to_about: nlh...false
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::spec_vals: a=-W <WUT2>
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=WUT2
[          clap::output::help] 	Help::align_to_about: arg=WUT2
[          clap::output::help] 	Help::align_to_about: Has switch...
[          clap::output::help] 	Yes
[          clap::output::help] 	Help::align_to_about: nlh...false
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_subcommands
[          clap::output::help] 	Help::write_subcommands longest = 4
[          clap::output::help] 	Help::sc_spec_vals: a=sub
[          clap::output::help] 	Help::sc_spec_vals: a=help
[          clap::output::help] 	Help::write_subcommand
[          clap::output::help] 	Help::sc_spec_vals: a=help
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_subcommand
[          clap::output::help] 	Help::sc_spec_vals: a=sub
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_after_help
[        clap::build::command] 	App::color: Color setting...
[        clap::build::command] 	Auto
test

USAGE:
    working [OPTIONS] <SUBCOMMAND>

OPTIONS:
    -h, --help       Print help information
    -W <WUT2>

SUBCOMMANDS:
    help    Print this message or the help of the given subcommand(s)
    sub
```

### src/deprecated-working.rs
```shell
[*]~/dev/claptest[master #]⦕ cargo run --bin deprecated-working
   Compiling claptest v0.1.0 (/Users/will/dev/claptest)
    Finished dev [unoptimized + debuginfo] target(s) in 0.55s
     Running `target/debug/deprecated-working`
CLAPTEST: test
Deprecated:
[        clap::build::command] 	App::_do_parse
[        clap::build::command] 	App::_build
[        clap::build::command] 	App::_propagate:test
[        clap::build::command] 	App::_check_help_and_version: test
[        clap::build::command] 	App::_check_help_and_version: Removing generated version
[        clap::build::command] 	App::_check_help_and_version: Building help subcommand
[        clap::build::command] 	App::_propagate_global_args:test
[        clap::build::command] 	App::_derive_display_order:test
[        clap::build::command] 	App::_derive_display_order:sub
[        clap::build::command] 	App::_derive_display_order:help
[  clap::build::debug_asserts] 	Command::_debug_asserts
[  clap::build::debug_asserts] 	Arg::_debug_asserts:help
[  clap::build::debug_asserts] 	Arg::_debug_asserts:WUT2
[  clap::build::debug_asserts] 	Command::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_env: Checking arg `--help`
[         clap::parse::parser] 	Parser::add_env: Checking arg `-W <WUT2>`
[         clap::parse::parser] 	Parser::add_env: Found an opt with value=RawOsStr("test"), trailing=false
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=WUT2, val=RawOsStr("test")
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."test"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[        clap::build::command] 	App::groups_for_arg: id=WUT2
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=WUT2
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:WUT2:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:WUT2: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:WUT2: doesn't have default missing vals
[      clap::parse::validator] 	Validator::new::get_matches_with: SubcommandRequiredElseHelp=true
[        clap::build::command] 	App::color: Color setting...
[        clap::build::command] 	Auto
[          clap::output::help] 	Help::new
[          clap::output::help] 	Help::write_help
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::write_templated_help
[          clap::output::help] 	Help::write_before_help
[          clap::output::help] 	Help::write_bin_name
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage] 	Usage::needs_options_tag
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=help
[         clap::output::usage] 	Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=WUT2
[        clap::build::command] 	App::groups_for_arg: id=WUT2
[         clap::output::usage] 	Usage::needs_options_tag:iter: [OPTIONS] required
[         clap::output::usage] 	Usage::create_help_usage: usage=deprecated-working [OPTIONS] <SUBCOMMAND>
[          clap::output::help] 	Help::write_all_args
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	should_show_arg: use_long=false, arg=WUT2
[          clap::output::help] 	Help::write_args
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::write_args: Current Longest...2
[          clap::output::help] 	Help::write_args: New Longest...6
[          clap::output::help] 	should_show_arg: use_long=false, arg=WUT2
[          clap::output::help] 	Help::write_args: Current Longest...6
[          clap::output::help] 	Help::write_args: New Longest...9
[          clap::output::help] 	should_show_arg: use_long=false, arg=help
[          clap::output::help] 	Help::spec_vals: a=--help
[          clap::output::help] 	should_show_arg: use_long=false, arg=WUT2
[          clap::output::help] 	Help::spec_vals: a=-W <WUT2>
[          clap::output::help] 	Help::spec_vals: Found environment variable...["CLAPTEST":Some("test")]
[          clap::output::help] 	Help::spec_vals: a=--help
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=help
[          clap::output::help] 	Help::align_to_about: arg=help
[          clap::output::help] 	Help::align_to_about: Has switch...
[          clap::output::help] 	Yes
[          clap::output::help] 	Help::align_to_about: nlh...false
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::spec_vals: a=-W <WUT2>
[          clap::output::help] 	Help::spec_vals: Found environment variable...["CLAPTEST":Some("test")]
[          clap::output::help] 	Help::short
[          clap::output::help] 	Help::long
[          clap::output::help] 	Help::val: arg=WUT2
[          clap::output::help] 	Help::align_to_about: arg=WUT2
[          clap::output::help] 	Help::align_to_about: Has switch...
[          clap::output::help] 	Yes
[          clap::output::help] 	Help::align_to_about: nlh...false
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_subcommands
[          clap::output::help] 	Help::write_subcommands longest = 4
[          clap::output::help] 	Help::sc_spec_vals: a=sub
[          clap::output::help] 	Help::sc_spec_vals: a=help
[          clap::output::help] 	Help::write_subcommand
[          clap::output::help] 	Help::sc_spec_vals: a=help
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_subcommand
[          clap::output::help] 	Help::sc_spec_vals: a=sub
[          clap::output::help] 	Help::help
[          clap::output::help] 	Help::help: Next Line...false
[          clap::output::help] 	Help::help: Too long...
[          clap::output::help] 	No
[          clap::output::help] 	Help::write_after_help
[        clap::build::command] 	App::color: Color setting...
[        clap::build::command] 	Auto
test

USAGE:
    deprecated-working [OPTIONS] <SUBCOMMAND>

OPTIONS:
    -h, --help       Print help information
    -W <WUT2>        [env: CLAPTEST=test]

SUBCOMMANDS:
    help    Print this message or the help of the given subcommand(s)
    sub
[*]~/dev/claptest[master #]⦕
```

---

_Label `C-bug` added by @willbuckner on 2022-03-24 11:58_

---

_Comment by @epage on 2022-03-24 14:29_

#3421 changed `arg_required_else_help` to ignore defaults to align with #3420 where all our requirement and conflict handling was updated to ignore defaults.  #3420 was a continuation of #3020 and #2950.

In #2950, we [discussed](https://github.com/clap-rs/clap/pull/2950#discussion_r736905259) how we should handle env variables and it was proposed that we treat them like user supplied values.

So the questions for this issue
- What is the ideal behavior of `arg_required_else_help` be when env variables are set and how important is that?
  - Show `--help` if there is no `ValueSource::CommandLine` value
    - Note: this would be a breaking change as this is dependent on the context while checking for defaults was not because that is static and turned a non-working case to a working case
  - Fallback to any errors associated with values being set to `ValueSource::EnvVariable`
  - Show `--help` if `is_subcommand_required` and there is no subcommand
- If we want to reconsider the handling of `ValueSource::EnvVariable` here, should we do it elsewhere?
  - Note: this would be a breaking change as this is dependent on the context while checking for defaults was not because that is static and turned a non-working case to a working case

@pksunkara since you were involved in the past discussion, I'd be curious to know what your thoughts are?  I think showing `--help` on a missing required subcommand is an easy choice but also wondering about the larger picture on this about env variables.

---

_Comment by @epage on 2022-03-24 14:58_

>  I think showing --help on a missing required subcommand is an easy choice but also wondering about the larger picture on this about env variables.

Got ahead of myself on this.  The intention was to be explicit about a subcommand being required when args are present but not a subcommand rather than the user having to infer that from the help message.

So if we did anything with this option, we'd have to explicitly check for if any CLI args are present.

---

_Label `A-help` added by @epage on 2022-03-24 14:59_

---

_Label `S-waiting-on-design` added by @epage on 2022-03-24 14:59_

---

_Comment by @willbuckner on 2022-03-24 17:04_

Thanks for the response!

>In https://github.com/clap-rs/clap/pull/2950, we https://github.com/clap-rs/clap/pull/2950#discussion_r736905259 how we should handle env variables and it was proposed that we treat them like user supplied values.

The best option might just be to let the user decide, which would also avoid a breaking change. Leave the default as it is, and add a `.env_satisfies_arg_requirements()` or `.env_satisfies_user_args()` (naming things is hard, but you get the idea :) ). I don't have an opinion as to whether this should be the _default_ behavior, but I just need some way to do it. A good compromise might be to leave the default behavior as is, have an opt-in setting, then flip the default for the breaking change in a major release if that's the consensus. 

The main issue right now is that I have no way to distinguish whether a user typed anything after `./mybinary` or not, including after `./mybinary subcommand` if my subcommand requires another subcommand. This effectively means I can't use Clap to use env values as defaults and I'll have to find another way to inject ENV vars. My usecase requires me to know whether or not the user manually specified any args.

In general, I'd also expect a user-supplied arg to take precedence over one provided from ENV, which I believe is the current behavior.

---

_Comment by @epage on 2022-03-28 19:12_

> The best option might just be to let the user decide, which would also avoid a breaking change. Leave the default as it is, and add a .env_satisfies_arg_requirements() or .env_satisfies_user_args() (naming things is hard, but you get the idea :) ). I don't have an opinion as to whether this should be the default behavior, but I just need some way to do it. A good compromise might be to leave the default behavior as is, have an opt-in setting, then flip the default for the breaking change in a major release if that's the consensus.

We are trying to limit the amount of configuration we provide.  The more configuration, the more cases we have to be documented and tested, and the worse our build times and code size are.  It also becomes overwhelming in the documentation, making it hard to be aware of what features clap has.  We are working on reducing the amount of configuration we have at this point.

> The main issue right now is that I have no way to distinguish whether a user typed anything after ./mybinary or not, including after ./mybinary subcommand if my subcommand requires another subcommand. This effectively means I can't use Clap to use env values as defaults and I'll have to find another way to inject ENV vars. My usecase requires me to know whether or not the user manually specified any args.

To double check, your concern is with what error we show the user?  The difference in error messages is enough to avoid that message?

---

_Comment by @willbuckner on 2022-03-29 02:00_

>To double check, your concern is with what error we show the user? The difference in error messages is enough to avoid that message?

No, the behavior I want (shown in expected output in the issue above) is for `mybinary mysubcommand` to show the full help for `mysubcommand`, rather than the usage message. I want the behavior we had before with `SubcommandRequiredElseHelp`. Now it shows a usage error, before it showed the help message.

>We are trying to limit the amount of configuration we provide. 

This makes sense and I definitely understand.

I just wish there was some way to achieve the previous functionality of `SubcommandRequiredElseHelp` without forgoing use of `.env()`. IMO, an env value is not a "user supplied argument". Really, I want to specifically require a subcommand, not just "any arg" or else show the help message (not the usage error). Thanks!

---

_Comment by @epage on 2022-03-29 18:47_

> No, the behavior I want (shown in expected output in the issue above) is for mybinary mysubcommand to show the full help for mysubcommand, rather than the usage message. I want the behavior we had before with SubcommandRequiredElseHelp. Now it shows a usage error, before it showed the help message.

Sorry, I was speaking for a clap internals perspective: those are both error messages.

---

_Renamed from ".arg_required_else_help() + .subcommand_required() does not replicate SubcommandRequiredElseHelp" to "`.arg_required_else_help().subcommand_required()` does not replicate SubcommandRequiredElseHelp with env variables" by @epage on 2022-05-04 18:18_

---

_Comment by @Kinrany on 2022-11-14 20:27_

I ran into this with the derive API. Now that the deprecated feature was removed in clap v4, it seems `Option<Command>` and a manual help printing is the only workaround.

Agree that when a required subcommand is missing, the list of possible subcommands should be shown. If this is a breaking change, adding a configuration option and changing the behavior in the next major version seems like the only path forward.

Edit: though perhaps missing or invalid arguments should have precedence over a missing subcommand. So that the CLI can be explored like this:

1. `foo` -> `error: --bar is required`
2. `foo --bar` -> `error: --bar needs a value`
3. `foo --bar baz` -> `help: list of all the possible subcommands`

This way at step 3 the user will know that they are passing the correct arguments and can proceed to choosing a subcommand.

---

_Comment by @epage on 2022-11-14 22:19_

> foo -> error: --bar is required

Mixing a required `--bar` with a required subcommand seems like a fairly uncommon case for us to optimize for.  I also feel like showing help is the more natural thing to do in this case.

> foo --bar baz -> help: list of all the possible subcommands

The user made a best effort and it still failed.  We should be more specific about what failed rather than sending them back to the beginning with help. We could prefix help with a message but that will most likely get lost in the noise.

---

_Comment by @Kinrany on 2022-11-15 00:54_

> Mixing a required --bar with a required subcommand seems like a fairly uncommon case for us to optimize for.

I'm thinking of environment variables, specifically database or other credentials needed for a collection of similar operations.

> The user made a best effort and it still failed.

Sorry, I meant that it shows the list of subcommands that can follow *after* `foo`, like `foo --bar baz fizz`.

Getting a help message with new information instead of an error seems like an intuitive sign of progress.

---

_Comment by @epage on 2022-11-15 02:38_

> Sorry, I meant that it shows the list of subcommands that can follow after foo, like foo --bar baz fizz.
>
> Getting a help message with new information instead of an error seems like an intuitive sign of progress.

Shows the list of subcommands or shows the help?  Maybe we are using terms slightly differently but its not clear to me what you are looking for.

As a possible guess as to what you mean, some similar cases
- When required arguments are missing, we show all required arguments to the user in the error message
- When a flag is missing a required value associated with it, we show all of the `PossibleValue`s in the error message

Similarly, for required subcommands, we could show all of the available subcommands in the error message when one is missing.

---

_Comment by @Kinrany on 2022-11-15 07:41_

Yeah, showing the list of subcommands in the error would fix the main problem: that the user needs to type another shell command just to look at the list.

Another reason I prefer help here is that it looks more consistent in a tree of subcommands: it's weird that between `foo`, `foo baz` and `foo baz fizz` the first and the last look the same but the middle one looks different because of an argument that was filled by an env var.

---

_Referenced in [clap-rs/clap#4482](../../clap-rs/clap/pulls/4482.md) on 2022-11-15 16:19_

---

_Comment by @litcc on 2022-11-23 01:42_

![image](https://user-images.githubusercontent.com/13202561/203453809-86dae4c9-3f79-42ee-9b97-5c75b99300e3.png)

a little different understanding here, when `arg_required_else_help(true)` is specified, theoretically the missing subcommand should display help information and exit normally.

When the env function is not used, he works normally and does show the help message, the
but when the env function is used, he just shows the available subcommands and does not show the help message, which I think is a logical problem because the user has specified the arg_required_else_help(true) argument and the subcommands are considered arg.


---

_Referenced in [clap-rs/clap#5113](../../clap-rs/clap/issues/5113.md) on 2023-09-06 16:05_

---

_Label `A-parsing` added by @epage on 2024-08-13 20:49_

---

_Renamed from "`.arg_required_else_help().subcommand_required()` does not replicate SubcommandRequiredElseHelp with env variables" to "`arg_required_else_help` does not show help when env variables are present" by @epage on 2024-08-13 20:50_

---

_Referenced in [clap-rs/clap#5673](../../clap-rs/clap/issues/5673.md) on 2024-08-13 20:53_

---

_Comment by @benluelo on 2024-11-02 00:06_

is there any progress here? this is quite annoying/frustrating to work around

---

_Comment by @epage on 2024-11-04 15:38_

If there is no posts, then there are no updates.

---
