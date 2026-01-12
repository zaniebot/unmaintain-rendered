```yaml
number: 5957
title: Exclusive options should work even when a required one is not present
type: issue
state: open
author: jaskij
labels:
  - C-bug
  - M-breaking-change
  - S-waiting-on-decision
  - A-derive
assignees: []
created_at: 2025-03-25T19:59:27Z
updated_at: 2025-03-28T17:36:30Z
url: https://github.com/clap-rs/clap/issues/5957
synced_at: 2026-01-12T16:14:17Z
```

# Exclusive options should work even when a required one is not present

---

_@jaskij_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.85.1 (4eb161250 2025-03-15)

### Clap Version

4.5.32

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser, Debug)]
struct Config {
    #[arg(long, exclusive(true))]
    pub(crate) build_details: bool,

    #[arg(short, long, required(true))]
    pub foo: i16,
}

fn main() {
    let config = Config::parse();
    println!("{}", config.build_details);
}
```


### Steps to reproduce the bug with the above code

```
cargo run -- --build-details
```

### Actual Behaviour

```
error: The following required argument was not provided: foo

Usage: clap-repro [OPTIONS] --foo <FOO>

For more information, try '--help'.
```

### Expected Behaviour

`true` is printed

### Additional Context

The docs for `exclusive()` state:

> Setting an exclusive argument and having any other arguments present at runtime is an error.

This would imply that other arguments, which are otherwise required, no longer are, and should not be present.

In fact, using an exclusive argument and a second, required, argument in the same CLI makes using the exclusive argument impossible.

Similar issues is also present with subcomannds, although there using `command subcommand --build-details` works.

---

Default features are disabled, and `clap` is added to the project with:

```toml
clap = { version = "4.0.28", default-features = false, features = ["color", "derive", "env", "error-context", "help", "string", "unicode", "usage", "std", "suggestions", "wrap_help"] }
```

---

I do remember this working as expected in the past.

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-repro"
[clap_builder::builder::command]Command::_propagate:clap-repro
[clap_builder::builder::command]Command::_check_help_and_version:clap-repro expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:clap-repro
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:build_details
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:foo
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--build-details"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--build-details")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--build-details'
[clap_builder::parser::parser]Parser::parse_long_arg("build-details"): Presence validated
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Parser::react: has default_missing_vals
[clap_builder::parser::parser]Parser::remove_overrides: id="build_details"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="build_details", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="build_details"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Config", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["true"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::get_matches_with: After parse_long_arg ValuesDone
[clap_builder::parser::parser]Parser::add_env
[clap_builder::parser::parser]Parser::add_env: Skipping existing arg `--build-details`
[clap_builder::parser::parser]Parser::add_env: Checking arg `--foo <FOO>`
[clap_builder::parser::parser]Parser::add_env: Checking arg `--help`
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:build_details:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:build_details: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:build_details: was used
[clap_builder::parser::parser]Parser::add_defaults:iter:foo:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:foo: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="build_details"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="build_details", conflicts=[]
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="Config", conflicts=[]
[clap_builder::parser::validator]Validator::validate_conflicts
[clap_builder::parser::validator]Validator::validate_exclusive
[clap_builder::parser::validator]Validator::validate_conflicts::iter: id="build_details"
[clap_builder::parser::validator]Conflicts::gather_conflicts: arg="build_details"
[clap_builder::parser::validator]Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]Validator::validate_required: required=ChildGraph([Child { id: "foo", children: [] }])
[clap_builder::parser::validator]Validator::gather_requires
[clap_builder::parser::validator]Validator::gather_requires:iter:"build_details"
[clap_builder::parser::validator]Validator::gather_requires:iter:"Config"
[clap_builder::parser::validator]Validator::gather_requires:iter:"Config":group
[clap_builder::parser::validator]Validator::validate_required: is_exclusive_present=true
[clap_builder::parser::validator]Validator::validate_required:iter:aog="foo"
[clap_builder::parser::validator]Validator::validate_required:iter: This is an arg
[clap_builder::parser::arg_matcher]ArgMatcher::get_global_values: global_arg_vec=[]
[clap_builder::builder::command]Command::_build: name="clap-repro"
[clap_builder::builder::command]Command::_propagate:clap-repro
[clap_builder::builder::command]Command::_check_help_and_version:clap-repro expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:clap-repro
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:build_details
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:foo
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::_build: name="clap-repro"
[clap_builder::builder::command]Command::_build: already built
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=build_details
[clap_builder::builder::command]Command::groups_for_arg: id="build_details"
[ clap_builder::output::usage]Usage::needs_options_tag:iter:iter: grp_s="Config"
[ clap_builder::output::usage]Usage::needs_options_tag:iter: [OPTIONS] required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["foo"]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: clap-repro [OPTIONS] --foo <FOO>
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: The following required argument was not provided: foo

Usage: clap-repro [OPTIONS] --foo <FOO>

For more information, try '--help'.
```

---

_Label `C-bug` added by @jaskij on 2025-03-25 19:59_

---

_Comment by @jaskij on 2025-03-25 20:01_

Having looked a little further, this behavior is also wrong according to the current (4.5.32) documentation for `conflicts_with()`.

---

_Label `A-derive` added by @epage on 2025-03-25 20:32_

---

_Comment by @epage on 2025-03-25 20:33_

The parser is working correctly.  The problem is when taking the parsed results and populating `Config` because there isn't a valid value to put in `Config::foo`.  If you change it to `Option<_>`, it works
```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { version = "4", features = ["derive"] }
---

use clap::Parser;

#[derive(Parser, Debug)]
struct Config {
    #[arg(long, exclusive(true))]
    pub(crate) build_details: bool,

    #[arg(short, long, required(true))]
    pub foo: Option<i16>,
}

fn main() {
    let config = Config::parse();
    println!("{}", config.build_details);
}
```

---

_Comment by @jaskij on 2025-03-25 22:22_

I see, and understand. In this case, having any non-`Option` required argument when `exclusive()` is used elsewhere is clearly a bug in user code, and the current output is, to me, confusing. It is the same output that would normally be used when the application user passed wrong arguments.

It's been quite a while since I had actually done anything more involved with clap, but it used to be that at least some errors in user code would trigger a different message, and the program would exit before usage validation. Maybe something like that would be possible here?

---

_Comment by @epage on 2025-03-26 14:32_

We do have debug asserts.  Those operate at the builder level.  The problem is we have "required but can be overriden" (`required = true`) and "required but fail if something may override this" (`i64`).  The debug asserts only know about the first and not the second.

I had considered making this case a panic (or maybe it originally was?).  I was concerned that it could be easy to miss until it gets into a users hands.  The question then was what provided a better user experience and I erred on the side of reporting it as a "required argument" error.

We could have the derive identify when `exclusive`, `conflicts_with`, etc are used and handle report a compilation error.  Some cases of this can't be detected and we have to deal with whether a partial solution helps enough users vs builds a false confidence making the trickier cases harder to debug.  This would also require duplicating more logic into the derive and keeping them in sync which gets messier.

It looks like we don't have this case documented.  If nothing else,  we should at least do that though I recognize documentation is a solution of last resort because people are unlikely to read it until they have a problem and matching users expectations for where to read something is difficult.

---

_Comment by @jaskij on 2025-03-27 02:08_

TLDR: my experience so far with clap has been panics on programmer errors, and the current behavior breaks the pattern, and hence the principle of least surprise. Instead of a panic, I get an almost-functional CLI.

---

> I had considered making this case a panic (or maybe it originally was?). I was concerned that it could be easy to miss until it gets into a users hands. The question then was what provided a better user experience and I erred on the side of reporting it as a "required argument" error.

While that would be a sound argument, *if* clap didn't panic on other programmer errors. As is, as clap's user, I *expect* it to generate a panic if I made a mistake building the CLI, and a different error is both confusing (hence this issue), and may not be caught for months if someone has weak tests (as I do).

>  The question then was what provided a better user experience and I erred on the side of reporting it as a "required argument" error.

That's a difficult question, because being a user interface library, you have two types of user experience to juggle: the programmer using clap, and the end-user.

> and matching users expectations for where to read something is difficult.

Hard agree here, especially with two API surfaces, and `docs.rs` format not being entirely conductive to guide-style documentation.

While on the topic: the documentation for `Args::exclusive()` does not directly mention `Args::conflicts_with()`. It can be inferred, especially if someone knows of `conflicts_with()`, but is not mentioned nor linked directly.

---

And continuing on documentation, personally I found [this documentation system](https://docs.divio.com/documentation-system/) a good read, at the very least in terms of mental framework and the fact that there *are* multiple types of documentations which go together, but are separate. 

---

_Label `M-breaking-change` added by @epage on 2025-03-28 17:25_

---

_Label `S-waiting-on-decision` added by @epage on 2025-03-28 17:25_

---

_Comment by @epage on 2025-03-28 17:36_

> TLDR: my experience so far with clap has been panics on programmer errors, and the current behavior breaks the pattern, and hence the principle of least surprise. Instead of a panic, I get an almost-functional CLI.

Yes.  We do this for
- debug asserts for commands being parsed
  - we offer a way to do this in tests across all commands
- on access of a field with `ArgMatches`

Those are a little easier to test for as you mostly need to run your parser and the problem is caught.  Here, its not just that you need to run your parser but you need very specific argument combinations.

It is worth reconsidering though with the confusion this has caused people.

> And continuing on documentation, personally I found [this documentation system](https://docs.divio.com/documentation-system/) a good read, at the very least in terms of mental framework and the fact that there are multiple types of documentations which go together, but are separate.

We offer
- Tutorial
- Refernece
- Cookbook (which is meant to fill the How To Guide role)

If you have concrete ideas on how to improve that organization, I'd be open.

---
