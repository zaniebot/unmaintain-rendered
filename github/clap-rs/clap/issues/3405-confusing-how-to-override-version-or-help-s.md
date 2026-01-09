---
number: 3405
title: "Confusing how to override `version` or `help`s behavior"
type: issue
state: closed
author: bcantrill
labels:
  - C-enhancement
  - M-breaking-change
  - A-builder
  - E-medium
assignees: []
created_at: 2022-02-05T04:39:11Z
updated_at: 2024-05-06T16:21:45Z
url: https://github.com/clap-rs/clap/issues/3405
synced_at: 2026-01-07T13:12:19-06:00
---

# Confusing how to override `version` or `help`s behavior

---

_Issue opened by @bcantrill on 2022-02-05 04:39_

Maintainer's notes
- We'd remove the pre-generated help / version arguments
  - Instead we'd add a version and help flag if needed.
  - This makes it so we have one way to customize
- We'd switch to an `Action` system where you set an arguments action to `Action::Set`, `Action::Extend`, `Action::SetTrue`, `Action::Count`, etc.  The ones relevant here would be `Action::Help` and `Action::Version`.
  - We'd default arguments to `Action::Set` or `Action::SetTrue` (depending on other settings) unless they were named `version` or `help` for which we'd default their actions to `Action::Help` and `Action::Version`
  - We can leverage Actions for also giving users control over short vs long help


Original title: "cannot have an option named "version""
- See `#[clap(global_setting(AppSettings::NoAutoVersion))]` for solving that
---
### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.59.0-nightly (34926f0a1 2021-12-22)

### Clap Version

3.0.14

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser)]                                                                  
#[clap(name = "foobar")]
pub struct Args {
    /// verbose messages
    #[clap(long, short)]
    pub verbose: bool,

    /// version the thing
    #[clap(long, short = 'x')]
    pub version: bool,
}

fn main() {
    let args = Args::parse();
    println!("verbose is {:?}, version is {:?}", args.verbose, args.version);
}
```


### Steps to reproduce the bug with the above code

`cargo run -- -x` or `cargo run -- --version`



### Actual Behaviour

```
foobar
```


### Expected Behaviour

```
verbose is false, version is true
```

### Additional Context

We need to have a `--version` option that does more than the Clap default.  Currently, Clap special-cases the string `version`, and seemingly overrides its behavior (changing the option name to anything else results in the expected behavior).  This seems to be a `version` manifestation of behavior similar to https://github.com/clap-rs/clap/issues/3403.

### Debug Output

```
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:foobar
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Removing generated version
[            clap::build::app] 	App::_propagate_global_args:foobar
[            clap::build::app] 	App::_derive_display_order:foobar
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:verbose
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:version
[clap::build::app::debug_asserts] 	App::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--version")' ([45, 45, 118, 101, 114, 115, 105, 111, 110])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("--version")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::parse_long_arg
[         clap::parse::parser] 	Parser::parse_long_arg: cur_idx:=1
[         clap::parse::parser] 	Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser] 	No
[         clap::parse::parser] 	Parser::parse_long_arg: Found valid opt or flag '--version'
[         clap::parse::parser] 	Parser::check_for_help_and_version_str
[         clap::parse::parser] 	Parser::check_for_help_and_version_str: Checking if --RawOsStr("version") is help or version...
[         clap::parse::parser] 	Version
[         clap::parse::parser] 	Parser::get_matches_with: After parse_long_arg VersionFlag
[         clap::parse::parser] 	Parser::version_err
[            clap::build::app] 	App::_render_version
[            clap::build::app] 	App::color: Color setting...
[            clap::build::app] 	Auto
foobar 
```

The debug output is very helpful about indicating what it's doing, it's just that this is behavior that we do not want.

---

_Label `C-bug` added by @bcantrill on 2022-02-05 04:39_

---

_Comment by @bcantrill on 2022-02-05 05:11_

For anyone else that runs across this: you can disable this behavior by adding this:

```
#[clap(global_setting(AppSettings::NoAutoVersion))]
```

Even though this behavior can be disabled, I would argue that the existing behavior violates the Principle of Least Surprise -- not least because it exits without providing any real indicator as to why. (I would also note that StructOpt seems to have had `no_version` that seems to not have made the transition to Clap 3.) 

---

_Comment by @epage on 2022-02-05 15:26_

The intent of the current behavior is to allow you to custmize the flag while still getting the same behavior.  We also offer customization by, behind the scenes, pre-creating some flags for you to modify with `mut_arg`.

That said, this is still not great. 

My current thought
- We'd remove the pre-generated help / version arguments
  - Instead we'd add a version and help flag if needed.
  - This makes it so we have one way to customize
- We'd switch to an `Action` system where you set an arguments action to `Action::Set`, `Action::Extend`, `Action::SetTrue`, `Action::Count`, etc.  The ones relevant here would be `Action::Help` and `Action::Version`.
  - We'd default arguments to `Action::Set` or `Action::SetTrue` (depending on other settings) unless they were named `version` or `help` for which we'd default their actions to `Action::Help` and `Action::Version`

The main concern is the discoverability of the defaults but I think this moves us in the right direction.

Going to rename this Issue to reflect the confusion over the existing API.

---

_Renamed from "cannot have an option named "version"" to "Confusing how to override `version` or `help`s behavior" by @epage on 2022-02-05 15:27_

---

_Label `C-bug` removed by @epage on 2022-02-05 15:27_

---

_Label `A-builder` added by @epage on 2022-02-05 15:27_

---

_Label `C-enhancement` added by @epage on 2022-02-05 15:27_

---

_Label `E-medium` added by @epage on 2022-02-05 15:27_

---

_Label `M-breaking-change` added by @epage on 2022-02-05 15:27_

---

_Comment by @bcantrill on 2022-02-06 01:11_

Totally agreed about the change in issue name (as one might guess, I had not yet discovered `NoAutoVersion` when I filed it!) and about the course of action.  Thank you -- and thank you for your work on Clap which has been a pleasure to use!


---

_Referenced in [clap-rs/clap#3425](../../clap-rs/clap/pulls/3425.md) on 2022-02-09 00:57_

---

_Referenced in [clap-rs/clap#2717](../../clap-rs/clap/issues/2717.md) on 2022-02-10 16:38_

---

_Referenced in [clap-rs/clap#3451](../../clap-rs/clap/pulls/3451.md) on 2022-02-11 18:51_

---

_Referenced in [clap-rs/clap#1015](../../clap-rs/clap/issues/1015.md) on 2022-04-18 12:20_

---

_Comment by @epage on 2022-05-09 21:54_

The change in pre-generated `help` and `version` cannot be done until clap 4.  We have the `unstable-v4` flag but I worry this would be too invasive of a change for that flag.  Might still be worth experimenting with.

I'd like to get Actions in for clap v3 users, independent of whats going on for clap 4.  In that case, we should probably do it first so we don't couple the design to any changes for v4.

Python has
- `store`: overwrite current entry
- `store_const`:  overwrite with a hardcoded value
- `store_true`: overwrite with `true`
- `store_false`: overwrite with `false`
- `append`
- `append_const`
- `count`: increment value stored
- `help`: display help
  - We'd probably want `Action::Help`, `Action::HelpShort`, and `Action::HelpLong`
- `version`: display version
  - Ditto about short and long
- `extend`
- custom ones by sub-classing`Action`

Python also includes a non-native action, `BooleanOptionalAction ` which implicitly creates a `--no-` variant and stores `true`/`false`.

Items like `store_true` and `count` can't be done until https://github.com/clap-rs/clap/issues/2683.

https://github.com/clap-rs/clap/issues/815 could be implemented like `BooleanOptionalAction` using  aliases without having to get into #3146.  We'd need to pass to the action what flag or arg was used.  This then helps with other cases like `Help`.

So the question is what can be supported without blocking on #2683 and is it worth going ahead without it?

Without inheritance exposed, removing of pre-generated help, or support for #2683, I'm thinking there isn't a use case that makes this worth it enough.  Going to focus on those other areas first.

---

_Added to milestone `4.0` by @epage on 2022-05-09 21:55_

---

_Referenced in [clap-rs/clap#3737](../../clap-rs/clap/issues/3737.md) on 2022-05-19 12:33_

---

_Referenced in [clap-rs/clap#3748](../../clap-rs/clap/issues/3748.md) on 2022-05-25 12:23_

---

_Referenced in [clap-rs/clap#2317](../../clap-rs/clap/issues/2317.md) on 2022-05-25 12:27_

---

_Comment by @epage on 2022-05-26 19:53_

To implement this sanely, I'm going to need to do a major parser refactor

Random notes
- With multiple values
  - Attached values (e.g. after `=`) disallow any further values
  - A delmited value (e.g. `bar,baz`) disallows any further values (so `foo bar,baz` is legal)

---

_Referenced in [clap-rs/clap#3765](../../clap-rs/clap/pulls/3765.md) on 2022-05-27 18:47_

---

_Referenced in [clap-rs/clap#3772](../../clap-rs/clap/issues/3772.md) on 2022-05-31 14:48_

---

_Referenced in [clap-rs/clap#3773](../../clap-rs/clap/pulls/3773.md) on 2022-05-31 19:11_

---

_Referenced in [clap-rs/clap#3774](../../clap-rs/clap/pulls/3774.md) on 2022-06-01 02:09_

---

_Comment by @epage on 2022-06-01 19:50_

Working on integrating actions (#3774, #3775, #3777) into the derive API and hitting a hurdle.
- Users are currently defining whatever type they want for storing occurrences (i8, u32, usize, etc)
- We'll define the value parser and `get_one::<T>` based on that type
- As-implemented, `Count` only supports `u64` for the stored type
- We want to support users providing their own value parsers for `Count` so they can do custom validation (range restrictions)

How do we support the user using whatever type they want like they can today?

Ideas
- ~~When no parser is specified, we could ask the action for a parser and then fallback to `value_types!`.~~
  - but we won't know what type to use with `get_one`
- We bake counting directly into `ValueParser` so we don't have to know about it
- `Count` supports multiple storage types, one for each integer type
- We key off of the type name to do custom logic for `Count`
  - This runs into issues with whether the user typed it in the way we expect (is it fully qualified?)
- We make each action a distinct type with `Action` just being a "box" for them (internally, just an enum).  We use deref specialization on the passed-in type to customize `Count`s behavior
  - If someone uses `Action` rather than `CountAction`, then we won't be able to help them
  - Really wish types could have associated types

---

_Referenced in [rust-lang/rustfmt#5395](../../rust-lang/rustfmt/issues/5395.md) on 2022-06-21 22:23_

---

_Referenced in [rust-lang/rustfmt#5396](../../rust-lang/rustfmt/pulls/5396.md) on 2022-06-21 23:04_

---

_Referenced in [clap-rs/clap#4056](../../clap-rs/clap/pulls/4056.md) on 2022-08-11 00:51_

---

_Closed by @epage on 2022-08-11 01:46_

---

_Referenced in [clap-rs/clap#4687](../../clap-rs/clap/issues/4687.md) on 2023-01-30 18:15_

---

_Comment by @thewh1teagle on 2024-05-06 16:21_

For anyone who look to override default version:

```rust
#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None, disable_version_flag = true)]
struct Args {
  ...
}
```

---
