```yaml
number: 4656
title: Can not use Vec-Flags with Environment Variables
type: issue
state: closed
author: MarcelCoding
labels:
  - C-bug
assignees: []
created_at: 2023-01-22T16:32:10Z
updated_at: 2023-01-23T18:29:10Z
url: https://github.com/clap-rs/clap/issues/4656
synced_at: 2026-01-12T16:14:16Z
```

# Can not use Vec-Flags with Environment Variables

---

_@MarcelCoding_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.66.0 (69f9c33d7 2022-12-12)

### Clap Version

4.1.1

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser, Debug)]
struct Args {
  #[arg(long, env = "IDS")]
  ids: Vec<i64>,
}

fn main() {
  let args = Args::parse();
  println!("{:?}", args);
}

```


### Steps to reproduce the bug with the above code

```
[marcel@pc-marcel rust-test]$ IDS=123,123 cargo run 
   Compiling clap v4.1.1
   Compiling rust-test v0.1.0 (/home/marcel/IdeaProjects/rust-test)
    Finished dev [unoptimized + debuginfo] target(s) in 3.59s
     Running `target/debug/rust-test`
error: invalid value '123,123' for '--ids <IDS>': invalid digit found in string

For more information, try '--help'.
```
```
[marcel@pc-marcel rust-test]$ IDS=123 cargo run 
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/rust-test`
Args { ids: [123] }
```

### Actual Behaviour

Lists can not be populated with multiple values, if the values are provided using environment variables.

### Expected Behaviour

Add an option to express lists in environment variables.

### Additional Context

_No response_

### Debug Output

```
[marcel@pc-marcel rust-test]$ IDS=123,123 cargo run 
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
     Running `target/debug/rust-test`
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="rust-test"
[      clap::builder::command]  Command::_propagate:rust-test
[      clap::builder::command]  Command::_check_help_and_version:rust-test expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_propagate_global_args:rust-test
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:ids
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::add_env
[        clap::parser::parser]  Parser::add_env: Checking arg `--ids <IDS>`
[        clap::parser::parser]  Parser::add_env: Found an opt with value="123,123"
[        clap::parser::parser]  Parser::react action=Append, identifier=None, source=EnvVariable
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id="ids", source=EnvVariable
[      clap::builder::command]  Command::groups_for_arg: id="ids"
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id="Args", source=EnvVariable
[        clap::parser::parser]  Parser::push_arg_values: ["123,123"]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=1
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
error: invalid value '123,123' for '--ids <IDS>': invalid digit found in string

For more information, try '--help'.
```
```
[marcel@pc-marcel rust-test]$ IDS=123 cargo run 
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
     Running `target/debug/rust-test`
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="rust-test"
[      clap::builder::command]  Command::_propagate:rust-test
[      clap::builder::command]  Command::_check_help_and_version:rust-test expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_propagate_global_args:rust-test
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:ids
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::add_env
[        clap::parser::parser]  Parser::add_env: Checking arg `--ids <IDS>`
[        clap::parser::parser]  Parser::add_env: Found an opt with value="123"
[        clap::parser::parser]  Parser::react action=Append, identifier=None, source=EnvVariable
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id="ids", source=EnvVariable
[      clap::builder::command]  Command::groups_for_arg: id="ids"
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id="Args", source=EnvVariable
[        clap::parser::parser]  Parser::push_arg_values: ["123"]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=1
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=ids, pending=0
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: expected=1, actual=0
[        clap::parser::parser]  Parser::react not enough values passed in, leaving it to the validator to complain
[        clap::parser::parser]  Parser::add_env: Checking arg `--help`
[        clap::parser::parser]  Parser::add_defaults
[        clap::parser::parser]  Parser::add_defaults:iter:ids:
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:ids: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:help:
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default vals
[     clap::parser::validator]  Validator::validate
[      clap::builder::command]  Command::groups_for_arg: id="ids"
[     clap::parser::validator]  Conflicts::gather_direct_conflicts id="ids", conflicts=[]
[     clap::parser::validator]  Conflicts::gather_direct_conflicts id="Args", conflicts=[]
[     clap::parser::validator]  Validator::validate_conflicts
[     clap::parser::validator]  Validator::validate_exclusive
[     clap::parser::validator]  Validator::validate_conflicts::iter: id="ids"
[     clap::parser::validator]  Conflicts::gather_conflicts: arg="ids"
[     clap::parser::validator]  Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator]  Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator]  Validator::gather_requires
[     clap::parser::validator]  Validator::gather_requires:iter:"ids"
[     clap::parser::validator]  Validator::gather_requires:iter:"Args"
[     clap::parser::validator]  Validator::gather_requires:iter:"Args":group
[     clap::parser::validator]  Validator::validate_required: is_exclusive_present=false
[   clap::parser::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[]
Args { ids: [123] }
```

---

_Label `C-bug` added by @MarcelCoding on 2023-01-22 16:32_

---

_Comment by @epage on 2023-01-23 14:14_

One option is you can set the [value_delimiter](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.value_delimiter).  This will also apply to the CLI.

---

_Comment by @MarcelCoding on 2023-01-23 18:29_

Ok, thanks, that would be fine for me. Sadly, I did not come up with this myself. But anyway, thanks!

---

_Closed by @MarcelCoding on 2023-01-23 18:29_

---
