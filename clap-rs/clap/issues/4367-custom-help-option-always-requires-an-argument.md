```yaml
number: 4367
title: Custom help option always requires an argument
type: issue
state: open
author: wfxr
labels:
  - C-bug
  - M-breaking-change
  - E-easy
  - A-derive
assignees: []
created_at: 2022-10-11T10:16:41Z
updated_at: 2022-10-12T12:03:19Z
url: https://github.com/clap-rs/clap/issues/4367
synced_at: 2026-01-12T16:14:15Z
```

# Custom help option always requires an argument

---

_@wfxr_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.66.0-nightly (a6b7274a4 2022-10-10)

### Clap Version

4.0.12

### Minimal reproducible code

```rust
use clap::{ArgAction, Parser};

#[derive(Parser)]
#[command(about, version)]
#[command(disable_help_flag = true)]
pub struct App {
    #[arg(short = 'h', long, default_value = "localhost")]
    pub host: String,

    #[arg(long, action = ArgAction::Help)]
    pub help: bool,
}

fn main() {
    App::parse();
}
```


### Steps to reproduce the bug with the above code

```sh
cargo run
```

### Actual Behaviour

The following message was printed:

```
error: The following required argument was not provided: help

Usage: clap-demo [OPTIONS]
```

### Expected Behaviour

No error printed.

### Additional Context

_No response_

### Debug Output

```
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="clap-demo"
[      clap::builder::command]  Command::_propagate:clap-demo
[      clap::builder::command]  Command::_check_help_and_version:clap-demo expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --version
[      clap::builder::command]  Command::_propagate_global_args:clap-demo
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:host
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Arg::_debug_asserts:version
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::add_defaults
[        clap::parser::parser]  Parser::add_defaults:iter:host:
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:host: has default vals
[        clap::parser::parser]  Parser::add_default_value:iter:host: wasn't used
[        clap::parser::parser]  Parser::react action=Set, identifier=None, source=DefaultValue
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id="host", source=DefaultValue
[      clap::builder::command]  Command::groups_for_arg: id="host"
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id="App", source=DefaultValue
[        clap::parser::parser]  Parser::push_arg_values: ["localhost"]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=1
[      clap::builder::command]  Command::groups_for_arg: id="host"
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=host, pending=0
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: expected=1, actual=0
[        clap::parser::parser]  Parser::react not enough values passed in, leaving it to the validator to complain
[        clap::parser::parser]  Parser::add_defaults:iter:help:
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:version:
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:version: doesn't have default vals
[     clap::parser::validator]  Validator::validate
[     clap::parser::validator]  Validator::validate_conflicts
[     clap::parser::validator]  Validator::validate_exclusive
[     clap::parser::validator]  Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator]  Validator::gather_requires
[     clap::parser::validator]  Validator::validate_required: is_exclusive_present=false
[   clap::parser::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[]
[      clap::builder::command]  Command::_build: name="clap-demo"
[      clap::builder::command]  Command::_propagate:clap-demo
[      clap::builder::command]  Command::_check_help_and_version:clap-demo expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --version
[      clap::builder::command]  Command::_propagate_global_args:clap-demo
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:host
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Arg::_debug_asserts:version
[clap::builder::debug_asserts]  Command::_verify_positionals
[      clap::builder::command]  Command::_build: name="clap-demo"
[      clap::builder::command]  Command::_build: already built
[         clap::output::usage]  Usage::create_usage_with_title
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::needs_options_tag
[         clap::output::usage]  Usage::needs_options_tag:iter: f=host
[      clap::builder::command]  Command::groups_for_arg: id="host"
[         clap::output::usage]  Usage::needs_options_tag:iter:iter: grp_s="App"
[         clap::output::usage]  Usage::needs_options_tag:iter: [OPTIONS] required
[         clap::output::usage]  Usage::get_args: incls=[]
[         clap::output::usage]  Usage::get_args: unrolled_reqs=[]
[         clap::output::usage]  Usage::get_args: ret_val=[]
[         clap::output::usage]  Usage::create_help_usage: usage=clap-demo [OPTIONS]
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
error: The following required argument was not provided: help

Usage: clap-demo [OPTIONS]
```

---

_Label `C-bug` added by @wfxr on 2022-10-11 10:16_

---

_Comment by @epage on 2022-10-11 15:00_

A workaround:
```rust
#!/usr/bin/env -S rust-script

//! ```cargo
//! [dependencies]
//! clap = { version = "=4.0.12", features = ["derive"] }
//! ```

use clap::{ArgAction, Parser};

#[derive(Parser)]
#[command(about, version)]
#[command(disable_help_flag = true)]
pub struct App {
    #[arg(short = 'h', long, default_value = "localhost")]
    pub host: String,

    #[arg(long, action = ArgAction::Help, default_value = "false")]
    pub help: bool,
}

fn main() {
    App::parse();
}
```

---

_Comment by @epage on 2022-10-11 15:02_

We probably should set the default value_parser, value, and missing value for this like other flags but I suspect that would be a breaking change.

I have also been considering adding support for `()` as a type to mean" don't bother reading it" which would be more appropriate.

---

_Label `M-breaking-change` added by @epage on 2022-10-11 15:02_

---

_Label `E-easy` added by @epage on 2022-10-11 15:02_

---

_Label `A-derive` added by @epage on 2022-10-11 15:02_

---

_Comment by @epage on 2022-10-11 15:09_

Except that workaround doesn't work because we take any value to mean to show help./

Bleh.  The joy of not realizing a test doesn't cover all cases

---

_Referenced in [clap-rs/clap#4371](../../clap-rs/clap/pulls/4371.md) on 2022-10-11 15:30_

---

_Comment by @epage on 2022-10-11 15:32_

#4371 will allow
```rust
#[test]
fn custom_help_flag() {
    #[derive(Debug, Clone, Parser)]
    #[command(disable_help_flag = true)]
    struct CliOptions {
        #[arg(short = 'h', long = "verbose-help", action = ArgAction::Help, value_parser = clap::value_parser!(bool))]
        help: (),
    }

    let result = CliOptions::try_parse_from(["cmd", "--verbose-help"]);
    let err = result.unwrap_err();
    assert_eq!(err.kind(), clap::error::ErrorKind::DisplayHelp);

    CliOptions::try_parse_from(["cmd"]).unwrap();
}
```

---

_Comment by @wfxr on 2022-10-12 02:46_

@epage Great! #4371 solves the problem perfectly. Thanks you!

---

_Closed by @wfxr on 2022-10-12 02:46_

---

_Comment by @epage on 2022-10-12 12:03_

Going to re-open as I feel like we should add a default value parser and value for Help and Version actions so they can be used with less ceremony

---

_Reopened by @epage on 2022-10-12 12:03_

---

_Added to milestone `5.0` by @epage on 2022-10-12 12:03_

---

_Referenced in [clap-rs/clap#4831](../../clap-rs/clap/issues/4831.md) on 2023-04-12 17:48_

---
