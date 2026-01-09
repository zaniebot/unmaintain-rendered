---
number: 3872
title: Environment variable sometimes overrides global arg, new in 3.2
type: issue
state: closed
author: pbevin
labels:
  - C-bug
assignees: []
created_at: 2022-06-26T14:59:44Z
updated_at: 2022-06-28T13:06:10Z
url: https://github.com/clap-rs/clap/issues/3872
synced_at: 2026-01-07T13:12:20-06:00
---

# Environment variable sometimes overrides global arg, new in 3.2

---

_Issue opened by @pbevin on 2022-06-26 14:59_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.61.0 (fe5b13d68 2022-05-18)

### Clap Version

3.2.6

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Debug, Parser)]
struct Opts {
    #[clap(short, long, global = true, env = "MY_ENV_VAR")]
    name: Option<String>,

    #[clap(subcommand)]
    command: Commands,
}

#[derive(Debug, Subcommand)]
enum Commands {
    Hello,
    Goodbye,
}

fn main() {
    let opts = Opts::parse();
    let name = opts.name.unwrap_or_else(|| "World".to_string());
    match opts.command {
        Commands::Hello => println!("Hello, {name}!"),
        Commands::Goodbye => println!("Goodbye, {name}!"),
    }
}


```


### Steps to reproduce the bug with the above code

```shell
MY_ENV_VAR=from_env cargo run -- --name from_arg hello
```

### Actual Behaviour

Prints "Hello, from_env!"

### Expected Behaviour

Prints "Hello, from_arg!"

### Additional Context

With clap 3.1.18, the output was "Hello, from_arg" in both these cases:

```
MY_ENV_VAR=from_env cargo run -- hello --name from_arg # "Hello, from_arg"
MY_ENV_VAR=from_env cargo run -- --name from_arg hello # "Hello, from_arg"
```

With clap 3.2.6, the output is different:
```
MY_ENV_VAR=from_env cargo run -- hello --name from_arg # "Hello, from_arg"
MY_ENV_VAR=from_env cargo run -- --name from_arg hello # "Hello, from_env"
```

### Debug Output

```toml
[dependencies]
clap = { version = "3.2.6", features = ["env", "derive", "debug"] }
```

```
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/clap3 --name from_arg hello`
[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="clap3"
[      clap::builder::command] 	Command::_propagate:clap3
[      clap::builder::command] 	Command::_check_help_and_version: clap3
[      clap::builder::command] 	Command::_check_help_and_version: Removing generated version
[      clap::builder::command] 	Command::_check_help_and_version: Building help subcommand
[      clap::builder::command] 	Command::_propagate_global_args:clap3
[      clap::builder::command] 	Command::_propagate removing hello's help
[      clap::builder::command] 	Command::_propagate pushing help to hello
[      clap::builder::command] 	Command::_propagate pushing name to hello
[      clap::builder::command] 	Command::_propagate removing goodbye's help
[      clap::builder::command] 	Command::_propagate pushing help to goodbye
[      clap::builder::command] 	Command::_propagate pushing name to goodbye
[      clap::builder::command] 	Command::_propagate removing help's help
[      clap::builder::command] 	Command::_propagate pushing help to help
[      clap::builder::command] 	Command::_propagate pushing name to help
[      clap::builder::command] 	Command::_derive_display_order:clap3
[      clap::builder::command] 	Command::_derive_display_order:hello
[      clap::builder::command] 	Command::_derive_display_order:goodbye
[      clap::builder::command] 	Command::_derive_display_order:help
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Arg::_debug_asserts:name
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--name")' ([45, 45, 110, 97, 109, 101])
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...1
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("--name")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::parse_long_arg
[        clap::parser::parser] 	Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser] 	Parser::parse_long_arg: Found valid arg or flag '--name <NAME>'
[        clap::parser::parser] 	Parser::parse_long_arg("name"): Found an arg with value 'None'
[        clap::parser::parser] 	Parser::parse_opt_value; arg=name, val=None, has_eq=false
[        clap::parser::parser] 	Parser::parse_opt_value; arg.settings=ArgFlags(GLOBAL | TAKES_VAL)
[        clap::parser::parser] 	Parser::parse_opt_value; Checking for val...
[        clap::parser::parser] 	Parser::parse_opt_value: More arg vals required...
[        clap::parser::parser] 	Parser::get_matches_with: After parse_long_arg Opt(name)
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("from_arg")' ([102, 114, 111, 109, 95, 97, 114, 103])
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...1
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser] 	Parser::split_arg_values; arg=name, val=RawOsStr("from_arg")
[        clap::parser::parser] 	Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=name, resolved=0, pending=1
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("hello")' ([104, 101, 108, 108, 111])
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...1
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("hello")
[        clap::parser::parser] 	Parser::get_matches_with: sc=Some("hello")
[        clap::parser::parser] 	Parser::parse_subcommand
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val={}
[      clap::builder::command] 	Command::_build_subcommand Setting bin_name of hello to "clap3 hello"
[      clap::builder::command] 	Command::_build_subcommand Setting display_name of hello to "clap3-hello"
[      clap::builder::command] 	Command::_build: name="hello"
[      clap::builder::command] 	Command::_propagate:hello
[      clap::builder::command] 	Command::_check_help_and_version: hello
[      clap::builder::command] 	Command::_check_help_and_version: Removing generated version
[      clap::builder::command] 	Command::_propagate_global_args:hello
[      clap::builder::command] 	Command::_derive_display_order:hello
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Arg::_debug_asserts:name
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::parse_subcommand: About to parse sc=hello
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::add_env
[        clap::parser::parser] 	Parser::add_env: Checking arg `--help`
[        clap::parser::parser] 	Parser::add_env: Checking arg `--name <NAME>`
[        clap::parser::parser] 	Parser::add_env: Found an opt with value=RawOsStr("from_env"), trailing=false
[        clap::parser::parser] 	Parser::split_arg_values; arg=name, val=RawOsStr("from_env")
[        clap::parser::parser] 	Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[        clap::parser::parser] 	Parser::react action=StoreValue, identifier=None, source=EnvVariable
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id=name, source=EnvVariable
[      clap::builder::command] 	Command::groups_for_arg: id=name
[        clap::parser::parser] 	Parser::push_arg_values: ["from_env"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[      clap::builder::command] 	Command::groups_for_arg: id=name
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=name, resolved=1, pending=0
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:name:
[        clap::parser::parser] 	Parser::add_default_value:iter:name: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:name: doesn't have default vals
[     clap::parser::validator] 	Validator::validate
[     clap::parser::validator] 	Validator::validate_conflicts
[     clap::parser::validator] 	Validator::validate_exclusive
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id=name
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg=name
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator] 	Validator::gather_requires
[     clap::parser::validator] 	Validator::gather_requires:iter:name
[     clap::parser::validator] 	Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator] 	Validator::validate_required_unless
[     clap::parser::validator] 	Validator::validate_matched_args
[     clap::parser::validator] 	Validator::validate_matched_args:iter:name: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            AnyValue {
                                inner: alloc::string::String,
                            },
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[     clap::parser::validator] 	Validator::validate_arg_num_vals
[     clap::parser::validator] 	Validator::validate_arg_values: arg="name"
[     clap::parser::validator] 	Validator::validate_arg_values: checking validator...
[     clap::parser::validator] 	good
[     clap::parser::validator] 	Validator::validate_arg_num_occurs: "name"=0
[        clap::parser::parser] 	Parser::resolve_pending: id=name
[        clap::parser::parser] 	Parser::react action=StoreValue, identifier=Some(Long), source=CommandLine
[        clap::parser::parser] 	Parser::react: cur_idx:=1
[        clap::parser::parser] 	Parser::remove_overrides: id=name
[   clap::parser::arg_matcher] 	ArgMatcher::start_occurrence_of_arg: id=name
[      clap::builder::command] 	Command::groups_for_arg: id=name
[        clap::parser::parser] 	Parser::push_arg_values: ["from_arg"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=2
[      clap::builder::command] 	Command::groups_for_arg: id=name
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=name, resolved=1, pending=0
[        clap::parser::parser] 	Parser::add_env
[        clap::parser::parser] 	Parser::add_env: Checking arg `--help`
[        clap::parser::parser] 	Parser::add_env: Skipping existing arg `--name <NAME>`
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:name:
[        clap::parser::parser] 	Parser::add_default_value:iter:name: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:name: doesn't have default vals
[     clap::parser::validator] 	Validator::validate
[     clap::parser::validator] 	Validator::validate_conflicts
[     clap::parser::validator] 	Validator::validate_exclusive
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id=name
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg=name
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator] 	Validator::gather_requires
[     clap::parser::validator] 	Validator::gather_requires:iter:name
[     clap::parser::validator] 	Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator] 	Validator::validate_required_unless
[     clap::parser::validator] 	Validator::validate_matched_args
[     clap::parser::validator] 	Validator::validate_matched_args:iter:name: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            AnyValue {
                                inner: alloc::string::String,
                            },
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[     clap::parser::validator] 	Validator::validate_arg_num_vals
[     clap::parser::validator] 	Validator::validate_arg_values: arg="name"
[     clap::parser::validator] 	Validator::validate_arg_values: checking validator...
[     clap::parser::validator] 	good
[     clap::parser::validator] 	Validator::validate_arg_num_occurs: "name"=1
[   clap::parser::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[help, name, help, name]
Hello, from_env!
```

---

_Label `C-bug` added by @pbevin on 2022-06-26 14:59_

---

_Comment by @epage on 2022-06-28 02:48_

Huh, this only seems to affect env and not defaults
```
#!/usr/bin/env -S rust-script --debug

//! ```cargo
//! [dependencies]
//! clap = { path = "../clap", features = ["env", "derive"] }
//! ```

use clap::{Parser, Subcommand};

#[derive(Debug, Parser)]
struct Opts {
    #[clap(short, long, global = true, default_value = "from_default")]
    name: String,

    #[clap(subcommand)]
    command: Commands,
}

#[derive(Debug, Subcommand)]
enum Commands {
    Hello,
    Goodbye,
}

fn main() {
    let opts = Opts::parse();
    let name = opts.name;
    match opts.command {
        Commands::Hello => println!("Hello, {name}!"),
        Commands::Goodbye => println!("Goodbye, {name}!"),
    }
}
```
```console
$ ./clap-3872.rs --name from_arg hello
Hello, from_arg!
```

---

_Referenced in [clap-rs/clap#3879](../../clap-rs/clap/pulls/3879.md) on 2022-06-28 03:41_

---

_Closed by @epage on 2022-06-28 13:01_

---

_Comment by @epage on 2022-06-28 13:06_

The fix is now available in v3.2.7

---
