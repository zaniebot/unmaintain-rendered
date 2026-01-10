---
number: 2484
title: Boolean argument accepts a value, even with takes_value = false
type: issue
state: closed
author: sjackman
labels:
  - C-bug
assignees: []
created_at: 2021-05-18T01:27:01Z
updated_at: 2021-05-18T09:24:33Z
url: https://github.com/clap-rs/clap/issues/2484
synced_at: 2026-01-10T01:27:19Z
---

# Boolean argument accepts a value, even with takes_value = false

---

_Issue opened by @sjackman on 2021-05-18 01:27_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.51.0 (2fd73fabe 2021-03-23)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

```rust
use clap::Clap;

#[derive(clap::Clap)]
struct Opts {
    /// Verbose logging
    #[clap(long, takes_value = false)]
    verbose: bool,
}

fn main() {
    println!("Value for verbose: {}", Opts::parse().verbose);
}
```


### Steps to reproduce the bug with the above code

```bash
cargo run -- --verbose=foobar
```

### Actual Behaviour

```console
$ cargo run
Value for verbose: false
$ cargo run -- --verbose
Value for verbose: true
$ cargo run -- --verbose=
Value for verbose: true
$ cargo run -- --verbose=foobar
Value for verbose: true
```

### Expected Behaviour

```console
$ cargo run
Value for verbose: false
$ cargo run -- --verbose
Value for verbose: true
$ cargo run -- --verbose=
Error: option '--verbose' doesn't accept an argument
$ cargo run -- --verbose=foobar
Error: option '--verbose' doesn't accept an argument
```

### Additional Context

_No response_

### Debug Output

```
$ cargo run -- --verbose=foobar
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:hello
[            clap::build::app] 	App::_derive_display_order:hello
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[clap::build::app::debug_asserts] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:verbose
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing '"--verbose=foobar"' ([45, 45, 118, 101, 114, 98, 111, 115, 101, 61, 102, 111, 111, 98, 97, 114])
[         clap::parse::parser] 	Parser::is_new_arg: "--verbose=foobar":NotFound
[         clap::parse::parser] 	Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser] 	Parser::is_new_arg: -- found
[         clap::parse::parser] 	Parser::is_new_arg: starts_new_arg=true
[         clap::parse::parser] 	Parser::possible_subcommand: arg="--verbose=foobar"
[         clap::parse::parser] 	Parser::get_matches_with: possible_sc=false, sc=None
[         clap::parse::parser] 	Parser::parse_long_arg
[         clap::parse::parser] 	Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser] 	Yes '"=foobar"'
[         clap::parse::parser] 	Parser::parse_long_arg: Found valid opt or flag '--verbose'
[         clap::parse::parser] 	Parser::check_for_help_and_version_str
[         clap::parse::parser] 	Parser::check_for_help_and_version_str: Checking if --"verbose" is help or version...
[         clap::parse::parser] 	Neither
[         clap::parse::parser] 	Parser::parse_flag
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of: arg=verbose
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of: first instance
[            clap::build::app] 	App::groups_for_arg: id=verbose
[         clap::parse::parser] 	Parser::get_matches_with: After parse_long_arg Flag(verbose)
[         clap::parse::parser] 	Parser::maybe_inc_pos_counter: arg = verbose
[         clap::parse::parser] 	Parser::maybe_inc_pos_counter: is it positional?
[         clap::parse::parser] 	No
[            clap::build::app] 	App::groups_for_arg: id=verbose
[         clap::parse::parser] 	Parser::remove_overrides
[         clap::parse::parser] 	Parser::remove_overrides:iter:verbose
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::validate_exclusive:iter:verbose
[      clap::parse::validator] 	Validator::gather_conflicts
[      clap::parse::validator] 	Validator::gather_conflicts:iter: id=verbose
[            clap::build::app] 	App::groups_for_arg: id=verbose
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::gather_requirements:iter:verbose
[      clap::parse::validator] 	Validator::validate_required_unless
[      clap::parse::validator] 	Validator::validate_matched_args
[      clap::parse::validator] 	Validator::validate_matched_args:iter:verbose: vals=[]
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="verbose"
[      clap::parse::validator] 	Validator::validate_arg_requires:"verbose"
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "verbose"=1
[    clap::parse::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[]
Value for verbose: true
```

---

_Label `T: bug` added by @sjackman on 2021-05-18 01:27_

---

_Comment by @sjackman on 2021-05-18 01:33_

Looks like this issue may be a duplicate of https://github.com/clap-rs/clap/issues/1543.

---

_Comment by @pksunkara on 2021-05-18 09:24_

Yup.

---

_Label `R: duplicate` added by @pksunkara on 2021-05-18 09:24_

---

_Closed by @pksunkara on 2021-05-18 09:24_

---
