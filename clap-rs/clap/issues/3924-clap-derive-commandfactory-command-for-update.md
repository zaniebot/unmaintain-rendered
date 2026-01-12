```yaml
number: 3924
title: "clap_derive CommandFactory::command_for_update implementations set required to true by default instead of true"
type: issue
state: closed
author: AndreasBackx
labels:
  - C-bug
assignees: []
created_at: 2022-07-13T15:15:12Z
updated_at: 2022-07-13T15:36:57Z
url: https://github.com/clap-rs/clap/issues/3924
synced_at: 2026-01-12T16:14:15Z
```

# clap_derive CommandFactory::command_for_update implementations set required to true by default instead of true

---

_@AndreasBackx_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.62.0 (a8314ef7d 2022-06-27)

### Clap Version

3.2.10

### Minimal reproducible code

```rust
use std::env;

use anyhow::Result;
use clap::Parser;
use clap::Subcommand;

#[derive(Parser)]
struct Cli {
    #[clap(subcommand)]
    command: Cmd,
}

#[derive(Subcommand)]
enum Cmd {
    Foo {
        #[clap(short, long)]
        bar: f32,
    },
}

fn main() -> Result<()> {
    eprintln!("--------------------------------------------------------------------------------");
    eprintln!("Below is the output when using command_for_update instead of command to get the");
    eprintln!("command from a CommandFactory in derive.");
    eprintln!("--------------------------------------------------------------------------------");
    eprintln!();

    match parse_mut() {
        Ok(_) => {
            println!("Everything good!");
        }
        Err(error) => {
            // Just printing instead of exiting to show differences.
            error.print()?;
        }
    }

    eprintln!();
    eprintln!("--------------------------------------------------------------------------------");
    eprintln!(
        "Below is when using command, the default when using Parser::parse."
    );
    eprintln!("--------------------------------------------------------------------------------");
    eprintln!();

    Cli::parse();
    Ok(())
}

fn parse_mut() -> std::result::Result<Cli, clap::Error> {
    let mut cmd = <Cli as clap::CommandFactory>::command_for_update();
    // We're ignoring the error here because it doesn't apply for our test case.
    let mut matches = cmd
        .try_get_matches_from_mut(&mut env::args_os())
        .map_err(|e| e.format(&mut cmd))?;
    <Cli as clap::FromArgMatches>::from_arg_matches_mut(&mut matches)
        .map_err(|e| e.format(&mut cmd))
}

```


### Steps to reproduce the bug with the above code

```
cargo run -- foo
```

### Actual Behaviour

```
--------------------------------------------------------------------------------
Below is the output when using command_for_update instead of command to get the
command from a CommandFactory in derive.
--------------------------------------------------------------------------------

error: The following required argument was not provided: bar

USAGE:
    clap_command_for_update_bug [SUBCOMMAND]

For more information try --help

--------------------------------------------------------------------------------
Below is when using command, the default when using Parser::parse.
--------------------------------------------------------------------------------

error: The following required arguments were not provided:
    --bar <BAR>

USAGE:
    clap_command_for_update_bug foo --bar <BAR>
```

clap_derive sets required to `false` in the generated code:
* https://github.com/clap-rs/clap/blob/ffd24af5fe1c8f2aacc7f709daac1e50f18ae4f2/clap_derive/src/derives/args.rs#L427-L441

The error does eventually get caught in the `from_arg_matches_mut` it seems, though then there's some context missing.

### Expected Behaviour

I'm not exactly "expecting" this, though I would love to know why it works like this internally.

```
--------------------------------------------------------------------------------
Below is the output when using command_for_update instead of command to get the
command from a CommandFactory in derive.
--------------------------------------------------------------------------------

error: The following required arguments were not provided:
    --bar <BAR>

USAGE:
    clap_command_for_update_bug foo --bar <BAR>

--------------------------------------------------------------------------------
Below is when using command, the default when using Parser::parse.
--------------------------------------------------------------------------------

error: The following required arguments were not provided:
    --bar <BAR>

USAGE:
    clap_command_for_update_bug foo --bar <BAR>
```

### Additional Context

We were using `CommandFactory::command_for_update` because we have a wrapper macro that adds some defaults. We didn't think twice about the difference between `command` and `command_for_update`, the mind kinda was blank and assumed "mutability" from its name. We fixed it by using `command` instead, though it would be neat if there was some more documentation on how this works and why there are two differences that seemingly return the same, though their implementation differs from what I see.

So I'm leaning towards this not being a bug. Though am reporting it nonetheless hoping to learn more about the internals and maybe we could add more documentation around these functions?

### Debug Output

```
--------------------------------------------------------------------------------
Below is the output when using command_for_update instead of command to get the
command from a CommandFactory in derive.
--------------------------------------------------------------------------------

[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="clap_command_for_update_bug"
[      clap::builder::command]  Command::_propagate:clap_command_for_update_bug
[      clap::builder::command]  Command::_check_help_and_version: clap_command_for_update_bug
[      clap::builder::command]  Command::_check_help_and_version: Removing generated version
[      clap::builder::command]  Command::_check_help_and_version: Building help subcommand
[      clap::builder::command]  Command::_propagate_global_args:clap_command_for_update_bug
[      clap::builder::command]  Command::_propagate removing foo's help
[      clap::builder::command]  Command::_propagate pushing help to foo
[      clap::builder::command]  Command::_propagate removing help's help
[      clap::builder::command]  Command::_propagate pushing help to help
[      clap::builder::command]  Command::_derive_display_order:clap_command_for_update_bug
[      clap::builder::command]  Command::_derive_display_order:foo
[      clap::builder::command]  Command::_derive_display_order:help
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("foo")' ([102, 111, 111])
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("foo")
[        clap::parser::parser]  Parser::get_matches_with: sc=Some("foo")
[        clap::parser::parser]  Parser::parse_subcommand
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage]  Usage::get_required_usage_from: ret_val={}
[      clap::builder::command]  Command::_build_subcommand Setting bin_name of foo to "clap_command_for_update_bug foo"
[      clap::builder::command]  Command::_build_subcommand Setting display_name of foo to "clap_command_for_update_bug-foo"
[      clap::builder::command]  Command::_build: name="foo"
[      clap::builder::command]  Command::_propagate:foo
[      clap::builder::command]  Command::_check_help_and_version: foo
[      clap::builder::command]  Command::_check_help_and_version: Removing generated version
[      clap::builder::command]  Command::_propagate_global_args:foo
[      clap::builder::command]  Command::_derive_display_order:foo
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:bar
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::parse_subcommand: About to parse sc=foo
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::add_defaults
[        clap::parser::parser]  Parser::add_defaults:iter:bar:
[        clap::parser::parser]  Parser::add_default_value:iter:bar: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:bar: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:help:
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default vals
[     clap::parser::validator]  Validator::validate
[     clap::parser::validator]  Validator::validate_conflicts
[     clap::parser::validator]  Validator::validate_exclusive
[     clap::parser::validator]  Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator]  Validator::gather_requires
[     clap::parser::validator]  Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator]  Validator::validate_required_unless
[     clap::parser::validator]  Validator::validate_matched_args
[        clap::parser::parser]  Parser::add_defaults
[        clap::parser::parser]  Parser::add_defaults:iter:help:
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default vals
[     clap::parser::validator]  Validator::validate
[     clap::parser::validator]  Validator::validate_conflicts
[     clap::parser::validator]  Validator::validate_exclusive
[     clap::parser::validator]  Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator]  Validator::gather_requires
[     clap::parser::validator]  Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator]  Validator::validate_required_unless
[     clap::parser::validator]  Validator::validate_matched_args
[   clap::parser::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[help, help]
[      clap::builder::command]  Command::_build: name="clap_command_for_update_bug"
[      clap::builder::command]  Command::_build: already built
[      clap::builder::command]  Command::_build: name="clap_command_for_update_bug"
[      clap::builder::command]  Command::_build: already built
[         clap::output::usage]  Usage::create_usage_with_title
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage]  Usage::get_required_usage_from: ret_val={}
[         clap::output::usage]  Usage::needs_options_tag
[         clap::output::usage]  Usage::needs_options_tag:iter: f=help
[         clap::output::usage]  Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage]  Usage::needs_options_tag: [OPTIONS] not required
[         clap::output::usage]  Usage::create_help_usage: usage=clap_command_for_update_bug [SUBCOMMAND]
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
error: The following required argument was not provided: bar

USAGE:
    clap_command_for_update_bug [SUBCOMMAND]

For more information try --help

--------------------------------------------------------------------------------
Below is when using command, the default when using Parser::parse.
--------------------------------------------------------------------------------

[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="clap_command_for_update_bug"
[      clap::builder::command]  Command::_propagate:clap_command_for_update_bug
[      clap::builder::command]  Command::_check_help_and_version: clap_command_for_update_bug
[      clap::builder::command]  Command::_check_help_and_version: Removing generated version
[      clap::builder::command]  Command::_check_help_and_version: Building help subcommand
[      clap::builder::command]  Command::_propagate_global_args:clap_command_for_update_bug
[      clap::builder::command]  Command::_propagate removing foo's help
[      clap::builder::command]  Command::_propagate pushing help to foo
[      clap::builder::command]  Command::_propagate removing help's help
[      clap::builder::command]  Command::_propagate pushing help to help
[      clap::builder::command]  Command::_derive_display_order:clap_command_for_update_bug
[      clap::builder::command]  Command::_derive_display_order:foo
[      clap::builder::command]  Command::_derive_display_order:help
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("foo")' ([102, 111, 111])
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("foo")
[        clap::parser::parser]  Parser::get_matches_with: sc=Some("foo")
[        clap::parser::parser]  Parser::parse_subcommand
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage]  Usage::get_required_usage_from: ret_val={}
[      clap::builder::command]  Command::_build_subcommand Setting bin_name of foo to "clap_command_for_update_bug foo"
[      clap::builder::command]  Command::_build_subcommand Setting display_name of foo to "clap_command_for_update_bug-foo"
[      clap::builder::command]  Command::_build: name="foo"
[      clap::builder::command]  Command::_propagate:foo
[      clap::builder::command]  Command::_check_help_and_version: foo
[      clap::builder::command]  Command::_check_help_and_version: Removing generated version
[      clap::builder::command]  Command::_propagate_global_args:foo
[      clap::builder::command]  Command::_derive_display_order:foo
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:bar
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::parse_subcommand: About to parse sc=foo
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::add_defaults
[        clap::parser::parser]  Parser::add_defaults:iter:bar:
[        clap::parser::parser]  Parser::add_default_value:iter:bar: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:bar: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:help:
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default vals
[     clap::parser::validator]  Validator::validate
[     clap::parser::validator]  Validator::validate_conflicts
[     clap::parser::validator]  Validator::validate_exclusive
[     clap::parser::validator]  Validator::validate_required: required=ChildGraph([Child { id: bar, children: [] }])
[     clap::parser::validator]  Validator::gather_requires
[     clap::parser::validator]  Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator]  Validator::validate_required:iter:aog=bar
[     clap::parser::validator]  Validator::validate_required:iter: This is an arg
[     clap::parser::validator]  Validator::is_missing_required_ok: bar
[     clap::parser::validator]  Conflicts::gather_conflicts: arg=bar
[     clap::parser::validator]  Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator]  Validator::missing_required_error; incl=[]
[     clap::parser::validator]  Validator::missing_required_error: reqs=ChildGraph([Child { id: bar, children: [] }])
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=true, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={bar}
[         clap::output::usage]  Usage::get_required_usage_from:iter:bar
[         clap::output::usage]  Usage::get_required_usage_from: ret_val={"--bar <BAR>"}
[     clap::parser::validator]  Validator::missing_required_error: req_args=[
    "--bar <BAR>",
]
[         clap::output::usage]  Usage::create_usage_with_title
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={bar}
[         clap::output::usage]  Usage::get_required_usage_from:iter:bar
[         clap::output::usage]  Usage::get_required_usage_from: ret_val={"--bar <BAR>"}
[         clap::output::usage]  Usage::needs_options_tag
[         clap::output::usage]  Usage::needs_options_tag:iter: f=bar
[         clap::output::usage]  Usage::needs_options_tag:iter Option is required
[         clap::output::usage]  Usage::needs_options_tag:iter: f=help
[         clap::output::usage]  Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage]  Usage::needs_options_tag: [OPTIONS] not required
[         clap::output::usage]  Usage::create_help_usage: usage=clap_command_for_update_bug foo --bar <BAR>
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
error: The following required arguments were not provided:
    --bar <BAR>

USAGE:
    clap_command_for_update_bug foo --bar <BAR>

For more information try --help
```

---

_Label `C-bug` added by @AndreasBackx on 2022-07-13 15:15_

---

_Comment by @epage on 2022-07-13 15:36_

To best understand the role of `command_update`, I recommend looking to the `Parse` trait as must of the other derive functions are written in support of the `Parse` trait:
```rust
    /// Update from iterator, exit on error
    fn update_from<I, T>(&mut self, itr: I)
    where
        I: IntoIterator<Item = T>,
        T: Into<OsString> + Clone,
    {
        let mut matches = <Self as CommandFactory>::command_for_update().get_matches_from(itr);
        let res = <Self as FromArgMatches>::update_from_arg_matches_mut(self, &mut matches)
            .map_err(format_error::<Self>);
        if let Err(e) = res {
            // Since this is more of a development-time error, we aren't doing as fancy of a quit
            // as `get_matches_from`
            e.exit()
        }
    }
```

`update_from` takes an existing `Cli` instance and updates it with a new command line.  

It is therefore intentional that fields that are required are no longer as we are modifying an existing instance that has those required fields set rather than initializing the struct for the first time.

Honestly, I'm not quite fully sold on the use case for this but it already existed and I've not had the heart to see how much impact there would be to rip it out.


---

_Closed by @epage on 2022-07-13 15:36_

---

_Referenced in [clap-rs/clap#3930](../../clap-rs/clap/issues/3930.md) on 2022-07-13 22:37_

---

_Referenced in [clap-rs/clap#4974](../../clap-rs/clap/issues/4974.md) on 2023-06-19 17:18_

---
