```yaml
number: 3293
title: "`SocketAddr` can not be parsed from env"
type: issue
state: closed
author: benthillerkus
labels:
  - C-bug
assignees: []
created_at: 2022-01-13T19:02:20Z
updated_at: 2022-01-13T19:06:30Z
url: https://github.com/clap-rs/clap/issues/3293
synced_at: 2026-01-12T16:14:14Z
```

# `SocketAddr` can not be parsed from env

---

_@benthillerkus_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.57.0 (f1edd0429 2021-11-29)

### Clap Version

3.0.6

### Minimal reproducible code

_`src/main.rs`_
```rust
use clap::Parser;
use std::net::SocketAddr;

#[derive(Parser)]
pub struct Args {
    #[clap(short)]
    address: SocketAddr,
}

fn main() {
    let args = Args::parse();

    println!("{}", args.address);
}

```

_`Cargo.toml`_

```TOML
[package]
edition = "2021"
name = "clap-socket-address-from-env"
version = "0.1.0"

[dependencies]
clap = {version = "3.0.6", features = ["derive", "env"]}
```

### Steps to reproduce the bug with the above code

1. Set environment variable `address`
    - For example in PowerShell as `$ENV:address = 127.0.0.1:1234`
    - Or use the dotenv crate to load the vars from a file before parsing
 2. `cargo run`

### Actual Behaviour

```
error: The following required arguments were not provided:
    -a <ADDRESS>

USAGE:
    clap-socket-address-from-env.exe -a <ADDRESS>

For more information try --help
error: process didn't exit successfully: `target\debug\clap-socket-address-from-env.exe` (exit code: 2)
```

### Expected Behaviour

```
127.0.0.1:1234
```

### Additional Context

- I'm on Windows 10 Pro 21H2 19044.1415
- `cargo run -- -a 127.0.0.1:1234` works as expected

### Debug Output

```
     Running `target\debug\clap-socket-address-from-env.exe`
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:clap-socket-address-from-env
[            clap::build::app]  App::_check_help_and_version
[            clap::build::app]  App::_check_help_and_version: Removing generated version
[            clap::build::app]  App::_propagate_global_args:clap-socket-address-from-env
[            clap::build::app]  App::_derive_display_order:clap-socket-address-from-env
[clap::build::app::debug_asserts]       App::_debug_asserts
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:help
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:address
[clap::build::app::debug_asserts]       App::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_env: Checking arg `--help`
[         clap::parse::parser]  Parser::add_env: Checking arg `-a <ADDRESS>`
[         clap::parse::parser]  Parser::add_defaults
[         clap::parse::parser]  Parser::add_defaults:iter:address:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:address: doesn't have default vals
[         clap::parse::parser]  Parser::add_value:iter:address: doesn't have default missing vals 
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::validate_required: required=ChildGraph([Child { id: address, children: [] }])
[      clap::parse::validator]  Validator::gather_requirements
[      clap::parse::validator]  Validator::validate_required:iter:aog=address
[      clap::parse::validator]  Validator::validate_required:iter: This is an arg
[      clap::parse::validator]  Validator::is_missing_required_ok: address
[      clap::parse::validator]  Validator::validate_arg_conflicts: a="address"
[      clap::parse::validator]  Validator::missing_required_error; incl=[]
[      clap::parse::validator]  Validator::missing_required_error: reqs=ChildGraph([Child { id: address, children: [] }])
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=true, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={address}
[         clap::output::usage]  Usage::get_required_usage_from:iter:address
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=["-a <ADDRESS>"]
[      clap::parse::validator]  Validator::missing_required_error: req_args=[
    "-a <ADDRESS>",
]
[         clap::output::usage]  Usage::create_usage_with_title
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={address}
[         clap::output::usage]  Usage::get_required_usage_from:iter:address
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=["-a <ADDRESS>"]
[         clap::output::usage]  Usage::needs_options_tag
[         clap::output::usage]  Usage::needs_options_tag:iter: f=help
[         clap::output::usage]  Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage]  Usage::needs_options_tag:iter: f=address
[         clap::output::usage]  Usage::needs_options_tag:iter Option is required
[         clap::output::usage]  Usage::needs_options_tag: [OPTIONS] not required
[         clap::output::usage]  Usage::create_help_usage: usage=clap-socket-address-from-env.exe -a <ADDRESS>
[            clap::build::app]  App::color: Color setting...
[            clap::build::app]  Auto
error: The following required arguments were not provided:
    -a <ADDRESS>

USAGE:
    clap-socket-address-from-env.exe -a <ADDRESS>

For more information try --help
error: process didn't exit successfully: `target\debug\clap-socket-address-from-env.exe` (exit code: 2)
```

---

_Label `C-bug` added by @benthillerkus on 2022-01-13 19:02_

---

_Comment by @benthillerkus on 2022-01-13 19:05_

Sorry, this was user error, forgot the env macro argument.


---

_Closed by @benthillerkus on 2022-01-13 19:05_

---
