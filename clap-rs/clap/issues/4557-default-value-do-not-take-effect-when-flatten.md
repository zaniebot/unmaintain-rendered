```yaml
number: 4557
title: default_value do not take effect  when flatten structs which have same struct field name C-bug
type: issue
state: closed
author: yiv
labels:
  - C-bug
assignees: []
created_at: 2022-12-15T12:52:49Z
updated_at: 2022-12-15T13:18:11Z
url: https://github.com/clap-rs/clap/issues/4557
synced_at: 2026-01-12T16:14:16Z
```

# default_value do not take effect  when flatten structs which have same struct field name C-bug

---

_@yiv_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.65.0 (897e37553 2022-11-02)

### Clap Version

4.0.29

### Minimal reproducible code

```rust
use clap::{command, Arg, Parser};

#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
struct AppConfig {
    #[clap(flatten)]
    redis: RedisConfig,
    #[clap(flatten)]
    database: DatabaseConfig,
}

#[derive(Parser, Debug)]
struct RedisConfig {
    #[arg(long = "redis.level", default_value = "")]
    name: String,
}


#[derive(Parser, Debug)]
struct DatabaseConfig {
    #[arg(long = "database.url", default_value = "")]
    name: String,
}

fn main() {
    let config = AppConfig::parse();
    println!("config {:?}", config);
}

```


### Steps to reproduce the bug with the above code

This issue is related with #4556, problem in #4556 also happen in this issue
The difference which is in this case using `default_value` to set default value for args whose name are all the same.

just run
```
cargo run --release
```

### Actual Behaviour

```shell
warning: `tt` (bin "tt") generated 1 warning
    Finished release [optimized] target(s) in 11.03s
     Running `target/release/tt`
error: The following required argument was not provided: name

Usage: tt [OPTIONS]

For more information try '--help'

```

### Expected Behaviour

should run successfully

### Additional Context

A crash error is reported when running development mode
```shell
cargo run
```

In this case, should running in release mode
```
cargo run --release
```

### Debug Output


warning: `tt` (bin "tt") generated 1 warning
    Finished release [optimized] target(s) in 11.03s
     Running `target/release/tt`
error: The following required argument was not provided: name

Usage: tt [OPTIONS]

For more information try '--help'
bytedance@MacBook-Pro-ED tt % cargo run --release
warning: unused import: `Arg`
 --> src/main.rs:1:21
  |
1 | use clap::{command, Arg, Parser};
  |                     ^^^
  |
  = note: `#[warn(unused_imports)]` on by default

warning: `tt` (bin "tt") generated 1 warning
    Finished release [optimized] target(s) in 0.16s
     Running `target/release/tt`
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="tt"
[      clap::builder::command]  Command::_propagate:tt
[      clap::builder::command]  Command::_check_help_and_version:tt expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_check_help_and_version: Building default --version
[      clap::builder::command]  Command::_propagate_global_args:tt
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::add_defaults
[        clap::parser::parser]  Parser::add_defaults:iter:name:
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:name: has default vals
[        clap::parser::parser]  Parser::add_default_value:iter:name: wasn't used
[        clap::parser::parser]  Parser::react action=Set, identifier=None, source=DefaultValue
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id="name", source=DefaultValue
[        clap::parser::parser]  Parser::push_arg_values: [""]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=1
[        clap::parser::parser]  Parser::add_defaults:iter:name:
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:name: has default vals
[        clap::parser::parser]  Parser::add_default_value:iter:name: was used
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
[      clap::builder::command]  Command::_build: name="tt"
[      clap::builder::command]  Command::_propagate:tt
[      clap::builder::command]  Command::_check_help_and_version:tt expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_check_help_and_version: Building default --version
[      clap::builder::command]  Command::_propagate_global_args:tt
[      clap::builder::command]  Command::_build: name="tt"
[      clap::builder::command]  Command::_build: already built
[         clap::output::usage]  Usage::create_usage_with_title
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::needs_options_tag
[         clap::output::usage]  Usage::needs_options_tag:iter: f=name
[      clap::builder::command]  Command::groups_for_arg: id="name"
[         clap::output::usage]  Usage::needs_options_tag:iter:iter: grp_s="RedisConfig"
[         clap::output::usage]  Usage::needs_options_tag:iter:iter: grp_s="DatabaseConfig"
[         clap::output::usage]  Usage::needs_options_tag:iter: [OPTIONS] required
[         clap::output::usage]  Usage::get_args: incls=[]
[         clap::output::usage]  Usage::get_args: unrolled_reqs=[]
[         clap::output::usage]  Usage::get_args: ret_val=[]
[         clap::output::usage]  Usage::create_help_usage: usage=tt [OPTIONS]
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
error: The following required argument was not provided: name

Usage: tt [OPTIONS]

For more information try '--help'


---

_Label `C-bug` added by @yiv on 2022-12-15 12:52_

---

_Comment by @epage on 2022-12-15 13:18_

Closing as release builds that panic during debug builds are undefined behavior.

---

_Closed by @epage on 2022-12-15 13:18_

---
