---
number: 6105
title: "Positional arguments can't conflict with each other"
type: issue
state: open
author: feraxhp
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2025-08-14T11:54:51Z
updated_at: 2025-08-15T13:50:29Z
url: https://github.com/clap-rs/clap/issues/6105
synced_at: 2026-01-07T13:12:20-06:00
---

# Positional arguments can't conflict with each other

---

_Issue opened by @feraxhp on 2025-08-14 11:54_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.88.0 (6b00bc388 2025-06-23)

### Clap Version

4.5.45

### Minimal reproducible code

```rust
use clap::{Arg, ArgGroup, ArgMatches, Command, arg, command};

fn main() {
    let cli = Command::new("clone")
        .aliases(["cl"])
        .about("Clone a repository from a configured platform")
        .args([
            // Definimos 'repo' como un argumento posicional que ocupa el primer slot (index 1)
            Arg::new("repo")
                .required(true)
                .help("The name of the repository or a single argument"),
            Arg::new("pconf")
                .num_args(2)
                .required(true)
                .value_names(["pconf", "url"])
                .conflicts_with("repo")
                .help("The URL or repository path"),
            // Arg::new("repo")
            //     .required(true)
            //     .help("The name of the repository or a single argument"),
        ])
        .group(
            ArgGroup::new("clone_target")
                .arg("pconf")
                .arg("repo")
                .required(true)
        );

    // --- Testing: clone somearg ---
    println!("--- Testing: clone somearg ---");
    let res1 = cli.clone().try_get_matches_from(vec!["clone", "somearg"]);

    match res1 {
        Ok(m) => {
            let s = m.get_one::<String>("repo");
            let p = m.get_one::<String>("pconf");
            // let u = m.get_one::<String>("url");

            println!("repo: {:?}", s); // Esperado: Some("somearg")
            println!("pconf: {:?}", p); // Esperado: None
            // println!("url_val: {:?}", u); // Esperado: None
        }
        Err(e) => {
            println!("Error: {}", e)
        }
    }

    // --- Testing: clone somearg butother ---
    println!("\n--- Testing: clone somearg butother ---");
    let res2 = cli
        .clone()
        .try_get_matches_from(vec!["clone", "l", "somearg", "butother"]);

    match res2 {
        Ok(m) => {
            let s = m.get_one::<String>("repo");
            let p = m.get_many::<String>("pconf");
            // let u = m.get_one::<String>("url");

            println!("repo: {:?}", s); // Esperado: None
            println!("pconf: {:?}", p); // Esperado: Some("somearg")
            // println!("url_val: {:?}", u); // Esperado: Some("butother")
        }
        Err(e) => {
            println!("Error: {}", e)
        }
    }

    // --- Testing: clone --- (should error because 'required(true)' on group)
    println!("\n--- Testing: clone (no args) ---");
    let res3 = cli.clone().try_get_matches_from(vec!["clone"]);
    match res3 {
        Ok(m) => {
            println!("Parsed successfully: {:?}", m);
        }
        Err(e) => {
            println!("Error: {}", e); // Esperado: Error, porque se requiere un argumento
        }
    }
}

```


### Steps to reproduce the bug with the above code

run `cargo run`

### Actual Behaviour

# Fails the second test

```output
--- Testing: clone somearg ---
repo: Some("somearg")
pconf: None

--- Testing: clone somearg butother ---
Error: error: the argument '<repo>' cannot be used with '<pconf> <url>'

Usage: clone <<pconf> <url>|repo>

For more information, try '--help'.


--- Testing: clone (no args) ---
Error: error: the following required arguments were not provided:
  <<pconf> <url>|repo>

Usage: clone <<pconf> <url>|repo>

For more information, try '--help'.
```

### Expected Behaviour

The second test should pass, because the 2 arguments are supplied.

### Additional Context

Well the idea is to have this behavior in the command line.

```bash
clone somearg // matches somearg as repo
clone somearg butother // matches somearg as pconf and butother as url
```

I know u can accomplish this by letting the first position argument to have 1.. amount of args and manage the logic after the match, but that is not ideal, because it does not allow you to have different value parsers for each case. 

### Debug Output

```
--- Testing: clone somearg ---
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clone"
[clap_builder::builder::command]Command::_propagate:clone
[clap_builder::builder::command]Command::_check_help_and_version:clone expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:clone
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:repo
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:pconf
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"somearg"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("somearg")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]Parser::resolve_pending: id="repo"
[clap_builder::parser::parser]Parser::react action=Set, identifier=Some(Index), source=CommandLine
[clap_builder::parser::parser]Parser::remove_overrides: id="repo"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="repo", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="repo"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="clone_target", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["somearg"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=repo, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:repo:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:repo: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:pconf:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:pconf: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="repo"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="repo", conflicts=["pconf"]
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="clone_target", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="repo"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="repo"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([Child { id: "repo", children: [] }, Child { id: "pconf", children: [] }, Child { id: "clone_target", children: [] }])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"repo"
[clap_builder::parser::validator]Validator::gather_requires:iter:"clone_target"
[clap_builder::parser::validator]Validator::gather_requires:iter:"clone_target":group
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::validator]Validator::validate_required:iter:aog="pconf"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::validator]Validator::is_missing_required_ok: pconf
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="pconf"
[clap_builder::builder::command]Command::groups_for_arg: id="pconf"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="pconf", conflicts=["repo", "repo"]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=["repo", "repo"]
[clap_builder::parser::validator]Validator::is_missing_required_ok: true (self)
[clap_builder::parser::arg_matcher]ArgMatcher::get_global_values: global_arg_vec=[]
repo: Some("somearg")
pconf: None

--- Testing: clone somearg butother ---
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clone"
[clap_builder::builder::command]Command::_propagate:clone
[clap_builder::builder::command]Command::_check_help_and_version:clone expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:clone
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:repo
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:pconf
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"l"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("l")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"somearg"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("somearg")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...2
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]Parser::resolve_pending: id="repo"
[clap_builder::parser::parser]Parser::react action=Set, identifier=Some(Index), source=CommandLine
[clap_builder::parser::parser]Parser::remove_overrides: id="repo"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="repo", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="repo"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="clone_target", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["l"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=repo, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"butother"'
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...2
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]Parser::resolve_pending: id="pconf"
[clap_builder::parser::parser]Parser::react action=Set, identifier=Some(Index), source=CommandLine
[clap_builder::parser::parser]Parser::remove_overrides: id="pconf"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="pconf", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="pconf"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="clone_target", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["somearg", "butother"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=3
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=pconf, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=2, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:repo:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:repo: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:pconf:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:pconf: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="repo"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="repo", conflicts=["pconf"]
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="clone_target", conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="pconf"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="pconf", conflicts=["repo", "repo"]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"repo"
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"clone_target"
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"pconf"
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="repo"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="repo"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=["pconf", "pconf"]
[clap_builder::parser::validator]Validator::build_conflict_err: name="repo"
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_smart_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::write_args: incls=["repo"]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["repo", "pconf", "clone_target"]
[clap_builder::builder::command]Command::unroll_args_in_group: group="clone_target"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="pconf"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="repo"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group: group="clone_target"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="pconf"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="repo"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[  clap_builder::builder::arg]Arg::name_no_brackets:pconf
[  clap_builder::builder::arg]Arg::name_no_brackets: val_names=[
    "pconf",
    "url",
]
[  clap_builder::builder::arg]Arg::name_no_brackets:repo
[  clap_builder::builder::arg]Arg::name_no_brackets: just name
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: clone <<pconf> <url>|repo>
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
Error: error: the argument '<repo>' cannot be used with '<pconf> <url>'

Usage: clone <<pconf> <url>|repo>

For more information, try '--help'.

Backtrace:
   0: backtrace::backtrace::win64::trace
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\backtrace-0.3.75\src\backtrace\win64.rs:85
      backtrace::backtrace::trace_unsynchronized<backtrace::capture::impl$4::create::closure_env$0>
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\backtrace-0.3.75\src\backtrace\mod.rs:66
   1: backtrace::backtrace::trace<backtrace::capture::impl$4::create::closure_env$0>
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\backtrace-0.3.75\src\backtrace\mod.rs:53
   2: backtrace::capture::Backtrace::create
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\backtrace-0.3.75\src\capture.rs:294
   3: backtrace::capture::Backtrace::new
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\backtrace-0.3.75\src\capture.rs:259
   4: clap_builder::error::Backtrace::new
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\error\mod.rs:913
   5: clap_builder::error::Error<clap_builder::error::format::RichFormatter>::new<clap_builder::error::format::RichFormatter>
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\error\mod.rs:140
   6: clap_builder::error::Error<clap_builder::error::format::RichFormatter>::argument_conflict<clap_builder::error::format::RichFormatter>
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\error\mod.rs:384
   7: clap_builder::parser::validator::Validator::build_conflict_err
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\parser\validator.rs:155
   8: clap_builder::parser::validator::Validator::validate_conflicts
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\parser\validator.rs:78
   9: clap_builder::parser::validator::Validator::validate
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\parser\validator.rs:54
  10: clap_builder::parser::parser::Parser::get_matches_with
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\parser\parser.rs:71
  11: clap_builder::builder::command::Command::_do_parse
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\builder\command.rs:4322
  12: clap_builder::builder::command::Command::try_get_matches_from_mut<alloc::vec::Vec<ref$<str$>,alloc::alloc::Global>,ref$<str$> >
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\builder\command.rs:902
  13: clap_builder::builder::command::Command::try_get_matches_from<alloc::vec::Vec<ref$<str$>,alloc::alloc::Global>,ref$<str$> >
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\builder\command.rs:812
  14: rust_::main
             at C:\Users\feraxhp\projects\tprojects\rust_\src\main.rs:50
  15: core::ops::function::FnOnce::call_once<void (*)(),tuple$<> >
             at C:\Users\feraxhp\.rustup\toolchains\stable-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:250
  16: core::hint::black_box
             at C:\Users\feraxhp\.rustup\toolchains\stable-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\hint.rs:482
      std::sys::backtrace::__rust_begin_short_backtrace<void (*)(),tuple$<> >
             at C:\Users\feraxhp\.rustup\toolchains\stable-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\sys\backtrace.rs:152
  17: std::rt::lang_start::closure$0<tuple$<> >
             at C:\Users\feraxhp\.rustup\toolchains\stable-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\rt.rs:199
  18: std::rt::lang_start_internal::closure$0
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library\std\src\rt.rs:168
      std::panicking::try::do_call
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library\std\src\panicking.rs:589
      std::panicking::try
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library\std\src\panicking.rs:552
      std::panic::catch_unwind
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library\std\src\panic.rs:359
      std::rt::lang_start_internal
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library\std\src\rt.rs:164
  19: std::rt::lang_start<tuple$<> >
             at C:\Users\feraxhp\.rustup\toolchains\stable-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\rt.rs:198
  20: main
  21: invoke_main
             at D:\a\_work\1\s\src\vctools\crt\vcstartup\src\startup\exe_common.inl:78
      __scrt_common_main_seh
             at D:\a\_work\1\s\src\vctools\crt\vcstartup\src\startup\exe_common.inl:288
  22: BaseThreadInitThunk
  23: RtlUserThreadStart



--- Testing: clone (no args) ---
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clone"
[clap_builder::builder::command]Command::_propagate:clone
[clap_builder::builder::command]Command::_check_help_and_version:clone expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:clone
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:repo
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:pconf
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:repo:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:repo: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:pconf:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:pconf: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([Child { id: "repo", children: [] }, Child { id: "pconf", children: [] }, Child { id: "clone_target", children: [] }])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::validator]Validator::validate_required:iter:aog="repo"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::validator]Validator::is_missing_required_ok: repo
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="repo"
[clap_builder::builder::command]Command::groups_for_arg: id="repo"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="repo", conflicts=["pconf"]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="repo"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="clone_target"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="clone_target", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "repo"
[clap_builder::parser::validator]Validator::validate_required:iter:aog="pconf"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::validator]Validator::is_missing_required_ok: pconf
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="pconf"
[clap_builder::builder::command]Command::groups_for_arg: id="pconf"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="pconf", conflicts=["repo", "repo"]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="pconf"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="clone_target"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="clone_target", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "pconf"
[clap_builder::parser::validator]Validator::validate_required:iter:aog="clone_target"
[clap_builder::parser::validator]Validator::validate_required:iter: This is a group
[clap_builder::builder::command]Command::unroll_args_in_group: group="clone_target"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="pconf"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="repo"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "clone_target"
[clap_builder::parser::validator]Validator::validate_required:iter: Missing "repo"
[clap_builder::parser::validator]Validator::missing_required_error; incl=["repo", "pconf", "clone_target", "repo"]
[clap_builder::parser::validator]Validator::missing_required_error: reqs=ChildGraph([Child { id: "repo", children: [] }, Child { id: "pconf", children: [] }, Child { id: "clone_target", children: [] }])
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=["repo", "pconf", "clone_target", "repo"], matcher=true, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=["repo", "pconf", "clone_target"]
[clap_builder::builder::command]Command::unroll_args_in_group: group="clone_target"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="pconf"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="repo"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"clone_target" group is_present=false
[clap_builder::builder::command]Command::unroll_args_in_group: group="clone_target"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="pconf"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="repo"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[  clap_builder::builder::arg]Arg::name_no_brackets:pconf
[  clap_builder::builder::arg]Arg::name_no_brackets: val_names=[
    "pconf",
    "url",
]
[  clap_builder::builder::arg]Arg::name_no_brackets:repo
[  clap_builder::builder::arg]Arg::name_no_brackets: just name
[clap_builder::builder::command]Command::unroll_args_in_group: group="clone_target"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="pconf"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="repo"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"clone_target" group is_present=false
[clap_builder::builder::command]Command::unroll_args_in_group: group="clone_target"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="pconf"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="repo"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[  clap_builder::builder::arg]Arg::name_no_brackets:pconf
[  clap_builder::builder::arg]Arg::name_no_brackets: val_names=[
    "pconf",
    "url",
]
[  clap_builder::builder::arg]Arg::name_no_brackets:repo
[  clap_builder::builder::arg]Arg::name_no_brackets: just name
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[StyledStr("<<pconf> <url>|repo>")]
[clap_builder::parser::validator]Validator::missing_required_error: req_args=[
    "<<pconf> <url>|repo>",
]
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_smart_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::write_args: incls=["repo", "pconf", "clone_target", "repo"]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["repo", "pconf", "clone_target"]
[clap_builder::builder::command]Command::unroll_args_in_group: group="clone_target"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="pconf"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="repo"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group: group="clone_target"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="pconf"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="repo"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[  clap_builder::builder::arg]Arg::name_no_brackets:pconf
[  clap_builder::builder::arg]Arg::name_no_brackets: val_names=[
    "pconf",
    "url",
]
[  clap_builder::builder::arg]Arg::name_no_brackets:repo
[  clap_builder::builder::arg]Arg::name_no_brackets: just name
[clap_builder::builder::command]Command::unroll_args_in_group: group="clone_target"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="pconf"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="repo"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group: group="clone_target"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="pconf"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="repo"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[  clap_builder::builder::arg]Arg::name_no_brackets:pconf
[  clap_builder::builder::arg]Arg::name_no_brackets: val_names=[
    "pconf",
    "url",
]
[  clap_builder::builder::arg]Arg::name_no_brackets:repo
[  clap_builder::builder::arg]Arg::name_no_brackets: just name
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: clone <<pconf> <url>|repo>
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
Error: error: the following required arguments were not provided:
  <<pconf> <url>|repo>

Usage: clone <<pconf> <url>|repo>

For more information, try '--help'.

Backtrace:
   0: backtrace::backtrace::win64::trace
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\backtrace-0.3.75\src\backtrace\win64.rs:85
      backtrace::backtrace::trace_unsynchronized<backtrace::capture::impl$4::create::closure_env$0>
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\backtrace-0.3.75\src\backtrace\mod.rs:66
   1: backtrace::backtrace::trace<backtrace::capture::impl$4::create::closure_env$0>
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\backtrace-0.3.75\src\backtrace\mod.rs:53
   2: backtrace::capture::Backtrace::create
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\backtrace-0.3.75\src\capture.rs:294
   3: backtrace::capture::Backtrace::new
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\backtrace-0.3.75\src\capture.rs:259
   4: clap_builder::error::Backtrace::new
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\error\mod.rs:913
   5: clap_builder::error::Error<clap_builder::error::format::RichFormatter>::new<clap_builder::error::format::RichFormatter>
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\error\mod.rs:140
   6: clap_builder::error::Error<clap_builder::error::format::RichFormatter>::missing_required_argument<clap_builder::error::format::RichFormatter>
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\error\mod.rs:557
   7: clap_builder::parser::validator::Validator::missing_required_error
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\parser\validator.rs:428
   8: clap_builder::parser::validator::Validator::validate_required
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\parser\validator.rs:338
   9: clap_builder::parser::validator::Validator::validate
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\parser\validator.rs:56
  10: clap_builder::parser::parser::Parser::get_matches_with
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\parser\parser.rs:71
  11: clap_builder::builder::command::Command::_do_parse
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\builder\command.rs:4322
  12: clap_builder::builder::command::Command::try_get_matches_from_mut<alloc::vec::Vec<ref$<str$>,alloc::alloc::Global>,ref$<str$> >
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\builder\command.rs:902
  13: clap_builder::builder::command::Command::try_get_matches_from<alloc::vec::Vec<ref$<str$>,alloc::alloc::Global>,ref$<str$> >
             at C:\Users\feraxhp\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\clap_builder-4.5.44\src\builder\command.rs:812
  14: rust_::main
             at C:\Users\feraxhp\projects\tprojects\rust_\src\main.rs:71
  15: core::ops::function::FnOnce::call_once<void (*)(),tuple$<> >
             at C:\Users\feraxhp\.rustup\toolchains\stable-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:250
  16: core::hint::black_box
             at C:\Users\feraxhp\.rustup\toolchains\stable-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\hint.rs:482
             at D:\a\_work\1\s\src\vctools\crt\vcstartup\src\startup\exe_common.inl:78
      __scrt_common_main_seh
             at D:\a\_work\1\s\src\vctools\crt\vcstartup\src\startup\exe_common.inl:288
  22: BaseThreadInitThunk
  23: RtlUserThreadStart
```


---

_Label `C-bug` added by @feraxhp on 2025-08-14 11:54_

---

_Renamed from "Exclusive group does not work with multiple" to "Positional arguments can't conflict with each other" by @epage on 2025-08-14 16:53_

---

_Label `A-parsing` added by @epage on 2025-08-14 16:53_

---

_Comment by @epage on 2025-08-14 16:57_

> Well the idea is to have this behavior in the command line.
> 
> ```
> clone somearg // matches somearg as repo
> clone somearg butother // matches somearg as pconf and butother as url
> ```

Clap does not support backtracking and trying alternative sets of arguments.  In fact, clap assigns positional indexes to each positional argument by default so `repo` has an index of `1` and `pconf` has an index of `2`.  Positional arguments are processed in index order.

---

_Comment by @feraxhp on 2025-08-15 00:43_

Yes, that's why i tried add `.index(1)` for `repo` and `pconf`. But it throws an error. 

---

_Comment by @epage on 2025-08-15 13:32_

Yes, as you found, cargo only allows one positional per index and not multiple.

---

_Comment by @epage on 2025-08-15 13:50_

Related issues
- #2122
- #1794
- #4837

---
