```yaml
number: 5776
title: conflicts_with_all creates a fully connected conflict network instead of pair-wise conflicts
type: issue
state: closed
author: nwalfield
labels:
  - C-bug
assignees: []
created_at: 2024-10-14T10:52:44Z
updated_at: 2024-10-14T13:16:37Z
url: https://github.com/clap-rs/clap/issues/5776
synced_at: 2026-01-12T16:14:17Z
```

# conflicts_with_all creates a fully connected conflict network instead of pair-wise conflicts

---

_@nwalfield_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.80.1 (3f5fd8dd4 2024-08-06)

### Clap Version

4.5.20

### Minimal reproducible code

```
use clap::Parser;
use clap::ArgGroup;

#[derive(Parser)]
#[command(version, about, long_about = None)]
#[clap(group(ArgGroup::new("constraints").args(&["regex", "domain", "unconstrained"]).required(true)))]
struct Cli {
    #[clap(
        long,
        value_name = "REGEX",
    )]
    regex: Vec<String>,
    #[clap(
        long,
        value_name = "DOMAIN",
    )]
    domain: Vec<String>,
    #[clap(
        long,
        help = "Opt out of any constraints.",
        conflicts_with_all = vec![ "regex", "domain" ],
    )]
    unconstrained: bool
}

fn main() {
    Cli::parse();
}
```

### Steps to reproduce the bug with the above code

```
$ cargo run -- --domain b --regex a
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/clap-issue --domain b --regex a`
error: the argument '--domain <DOMAIN>' cannot be used with '--regex <REGEX>'

Usage: clap-issue <--regex <REGEX>|--domain <DOMAIN>|--unconstrained>

For more information, try '--help'.
```

### Actual Behaviour

I can only provide `--domain` arguments or `--regex` arguments, but not a mix.

### Expected Behaviour

Any number of `--domain` or `--regex` arguments should be accepted.  Currently it is possible to specify any number of domain arguments OR regex arguments, but not a mix.  `--unconstrained` should conflict with `--domain` and `--regex`, but `--domain` and `--regex` should not conflict with each other.

### Additional Context

We have a command that accepts zero or more constraints (`--domain` or `--regex`).  Since not providing any constraints is dangerous, we wanted to force the user to explicitly opt in to not using any constraints using the `--unconstrained` flag.

I thought that the above code would work since [`conflicts_with` is two way](https://docs.rs/clap/latest/clap/struct.Arg.html#method.conflicts_with).  But `conflicts_with` instead seems to derive a fully connected graph.  That is, `--unconstrained` and `--domain` should conflict, `--unconstrained` and `--regex` should conflict, but `--domain` and `--regex` shouldn't.

### Debug Output

```
$ cargo run -- --domain b --domain a
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/clap-issue --domain b --domain a`
us@alice:/tmp/clap-issue$ cargo run -- --domain b --regex a
     Locking 10 packages to latest compatible versions
      Adding addr2line v0.24.2
      Adding adler2 v2.0.0
      Adding backtrace v0.3.74
      Adding cfg-if v1.0.0
      Adding gimli v0.31.1
      Adding libc v0.2.159
      Adding memchr v2.7.4
      Adding miniz_oxide v0.8.0
      Adding object v0.36.5
      Adding rustc-demangle v0.1.24
  Downloaded addr2line v0.24.2
  Downloaded gimli v0.31.1
  Downloaded object v0.36.5
  Downloaded 3 crates (646.0 KB) in 0.55s
   Compiling libc v0.2.159
   Compiling gimli v0.31.1
   Compiling memchr v2.7.4
   Compiling adler2 v2.0.0
   Compiling cfg-if v1.0.0
   Compiling rustc-demangle v0.1.24
   Compiling clap_derive v4.5.18
   Compiling miniz_oxide v0.8.0
   Compiling object v0.36.5
   Compiling addr2line v0.24.2
   Compiling backtrace v0.3.74
   Compiling clap_builder v4.5.20
   Compiling clap v4.5.20
   Compiling clap-issue v0.1.0 (/tmp/clap-issue)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 3.27s
     Running `target/debug/clap-issue --domain b --regex a`
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-issue"
[clap_builder::builder::command]Command::_propagate:clap-issue
[clap_builder::builder::command]Command::_check_help_and_version:clap-issue expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_propagate_global_args:clap-issue
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:regex
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:domain
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:unconstrained
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:version
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--domain"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--domain")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--domain <DOMAIN>'
[clap_builder::parser::parser]Parser::parse_long_arg("domain"): Found an arg with value 'None'
[clap_builder::parser::parser]Parser::parse_opt_value; arg=domain, val=None, has_eq=false
[clap_builder::parser::parser]Parser::parse_opt_value; arg.settings=ArgFlags(0)
[clap_builder::parser::parser]Parser::parse_opt_value; Checking for val...
[clap_builder::parser::parser]Parser::parse_opt_value: More arg vals required...
[clap_builder::parser::parser]Parser::get_matches_with: After parse_long_arg Opt("domain")
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"b"'
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=domain, pending=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=1
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--regex"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--regex")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--regex <REGEX>'
[clap_builder::parser::parser]Parser::parse_long_arg("regex"): Found an arg with value 'None'
[clap_builder::parser::parser]Parser::parse_opt_value; arg=regex, val=None, has_eq=false
[clap_builder::parser::parser]Parser::parse_opt_value; arg.settings=ArgFlags(0)
[clap_builder::parser::parser]Parser::parse_opt_value; Checking for val...
[clap_builder::parser::parser]Parser::parse_opt_value: More arg vals required...
[clap_builder::parser::parser]Parser::resolve_pending: id="domain"
[clap_builder::parser::parser]Parser::react action=Append, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Parser::react: cur_idx:=1
[clap_builder::parser::parser]Parser::remove_overrides: id="domain"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="domain", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="domain"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Cli", source=CommandLine
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="constraints", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["b"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=2
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=domain, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::get_matches_with: After parse_long_arg Opt("regex")
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"a"'
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=regex, pending=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=1
[clap_builder::parser::parser]Parser::resolve_pending: id="regex"
[clap_builder::parser::parser]Parser::react action=Append, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Parser::react: cur_idx:=3
[clap_builder::parser::parser]Parser::remove_overrides: id="regex"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="regex", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="regex"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Cli", source=CommandLine
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="constraints", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["a"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=4
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=regex, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:regex:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:regex: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:domain:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:domain: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:unconstrained:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:unconstrained: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:unconstrained: wasn't used
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=None, source=DefaultValue
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="unconstrained", source=DefaultValue
[clap_builder::parser::parser]Parser::push_arg_values: ["false"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=5
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:version:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:version: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="domain"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="domain", conflicts=["regex", "unconstrained"]
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="Cli", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="constraints", conflicts=[]
[clap_builder::builder::command]Command::groups_for_arg: id="regex"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="regex", conflicts=["domain", "unconstrained"]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"domain"
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"Cli"
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"constraints"
[clap_builder::parser::validator]Validator::validate_exclusive:iter:"regex"
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="domain"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="domain"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=["regex", "regex"]
[clap_builder::parser::validator]Validator::build_conflict_err: name="domain"
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_smart_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::write_args: incls=["domain"]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["constraints"]
[clap_builder::builder::command]Command::unroll_args_in_group: group="constraints"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="regex"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="domain"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="unconstrained"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group: group="constraints"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="regex"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="domain"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[clap_builder::builder::command]Command::unroll_args_in_group:iter: entity="unconstrained"
[clap_builder::builder::command]Command::unroll_args_in_group:iter: this is an arg
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: clap-issue <--regex <REGEX>|--domain <DOMAIN>|--unconstrained>
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: the argument '--domain <DOMAIN>' cannot be used with '--regex <REGEX>'

Usage: clap-issue <--regex <REGEX>|--domain <DOMAIN>|--unconstrained>

For more information, try '--help'.

```

---

_Label `C-bug` added by @nwalfield on 2024-10-14 10:52_

---

_Comment by @nwalfield on 2024-10-14 13:16_

I figured out the issue.  Even though `domain` and `regex` implicitly have `multiple(true)`, the argument group does not.  Concretely, the following works:

```
#[clap(group(ArgGroup::new("constraints").args(&["regex", "domain", "unconstrained"]).required(true).multiple(true)))]
```

Sorry for the noise!

---

_Closed by @nwalfield on 2024-10-14 13:16_

---
