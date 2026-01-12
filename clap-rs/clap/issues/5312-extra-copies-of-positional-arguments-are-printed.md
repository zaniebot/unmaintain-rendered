```yaml
number: 5312
title: Extra copies of positional arguments are printed in error message
type: issue
state: closed
author: Fogapod
labels:
  - C-bug
assignees: []
created_at: 2024-01-16T08:30:52Z
updated_at: 2024-01-16T20:26:42Z
url: https://github.com/clap-rs/clap/issues/5312
synced_at: 2026-01-12T16:14:16Z
```

# Extra copies of positional arguments are printed in error message

---

_@Fogapod_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.75.0 (82e1608df 2023-12-21)

### Clap Version

4.4.17

### Minimal reproducible code

```rust
use clap::Parser;
use clap::Subcommand;

#[derive(Parser)]
struct Args {
    #[command(subcommand)]
    subcommand: Sub,
}

#[derive(Parser)]
struct ExpireSub {
    filename: String,
    token: String,
    expires: u64,
}

#[derive(Parser)]
struct DeleteSub {
    filename: String,
    token: String,
}

#[derive(Subcommand)]
enum Sub {
    Expire(ExpireSub),
    Delete(DeleteSub),
}

fn main() {
    let _args = Args::parse();
}
```

I also tried using `Args` instead of `Parser` for subcommands but result is the same.

### Steps to reproduce the bug with the above code

```
cargo run -- delete
cargo run -- expire
cargo run -- expire 1
```

### Actual Behaviour

```rust
// delete
error: the following required arguments were not provided:
  <FILENAME>
  <TOKEN>
  <FILENAME>   <------ extra

// expire with 0 of 3 args
error: the following required arguments were not provided:
  <FILENAME>
  <TOKEN>
  <EXPIRES>
  <FILENAME>   <------ extra
  <TOKEN>      <------ extra

// expire with 1 of 3 args
error: the following required arguments were not provided:
  <TOKEN>
  <EXPIRES>
  <TOKEN>      <------ extra
```

### Expected Behaviour

```rust
// delete
error: the following required arguments were not provided:
  <FILENAME>
  <TOKEN>

// expire with 0 of 3 args
error: the following required arguments were not provided:
  <FILENAME>
  <TOKEN>
  <EXPIRES>

// expire with 1 of 3 args
error: the following required arguments were not provided:
  <TOKEN>
  <EXPIRES>
```

### Additional Context

My clap features: `clap = { version = "4.4.16", default-features = false, features = ["std", "error-context", "help", "derive"] }`

### Debug Output

<details>
  <summary>Spoiler</summary>

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="null-pointer"
[clap_builder::builder::command]Command::_propagate:null-pointer
[clap_builder::builder::command]Command::_check_help_and_version:null-pointer expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:null-pointer
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"expire"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("expire")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("expire")
[clap_builder::parser::parser]Parser::parse_subcommand
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of expire to "0x0 expire"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of expire to "null-pointer-expire"
[clap_builder::builder::command]Command::_build: name="expire"
[clap_builder::builder::command]Command::_propagate:expire
[clap_builder::builder::command]Command::_check_help_and_version:expire expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:expire
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:filename
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:token
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:expires
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=expire
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:filename:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:filename: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:token:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:token: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:expires:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:expires: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([Child { id: "filename", children: [] }, Child { id: "token", children: [] }, Child { id: "expires", children: [] }])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::validator]Validator::validate_required:iter:aog="filename"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::validator]Validator::is_missing_required_ok: filename
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="filename"
[clap_builder::builder::command]Command::groups_for_arg: id="filename"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="filename", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="filename"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="ExpireSub"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="ExpireSub", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "filename"
[clap_builder::parser::validator]Validator::validate_required:iter:aog="token"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::validator]Validator::is_missing_required_ok: token
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="token"
[clap_builder::builder::command]Command::groups_for_arg: id="token"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="token", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="token"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="ExpireSub"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="ExpireSub", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "token"
[clap_builder::parser::validator]Validator::validate_required:iter:aog="expires"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::validator]Validator::is_missing_required_ok: expires
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="expires"
[clap_builder::builder::command]Command::groups_for_arg: id="expires"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="expires", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="expires"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="ExpireSub"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="ExpireSub", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "expires"
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "filename"
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "token"
[clap_builder::parser::validator]Validator::missing_required_error; incl=["filename", "token", "expires", "filename", "token"]
[clap_builder::parser::validator]Validator::missing_required_error: reqs=ChildGraph([Child { id: "filename", children: [] }, Child { id: "token", children: [] }, Child { id: "expires", children: [] }])
[clap_builder::parser::validator]Validator::missing_required_error: req_args=[
    "<FILENAME>",
    "<TOKEN>",
    "<EXPIRES>",
    "<FILENAME>",
    "<TOKEN>",
]
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Command::color: Color setting...
```
</details>

---

_Label `C-bug` added by @Fogapod on 2024-01-16 08:30_

---

_Referenced in [clap-rs/clap#5314](../../clap-rs/clap/pulls/5314.md) on 2024-01-16 20:16_

---

_Closed by @epage on 2024-01-16 20:26_

---
