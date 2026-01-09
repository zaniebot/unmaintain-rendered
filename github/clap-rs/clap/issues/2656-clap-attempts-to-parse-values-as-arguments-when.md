---
number: 2656
title: "Clap attempts to parse values as arguments when they begin with a `-` and are quoted"
type: issue
state: closed
author: alpha-tango-kilo
labels:
  - C-bug
assignees: []
created_at: 2021-08-02T15:51:52Z
updated_at: 2021-08-02T15:56:03Z
url: https://github.com/clap-rs/clap/issues/2656
synced_at: 2026-01-07T13:12:19-06:00
---

# Clap attempts to parse values as arguments when they begin with a `-` and are quoted

---

_Issue opened by @alpha-tango-kilo on 2021-08-02 15:51_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.54.0 (a178d0322 2021-07-26)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

```rust
use clap::{App, Arg};

fn main() {
    let app =
        App::new(env!("CARGO_PKG_NAME")).arg(Arg::new("year_range").short('y').takes_value(true));
	let _matches = app
            .try_get_matches()
            .expect("Parses successfully");
}
```

### Steps to reproduce the bug with the above code

`cargo run -- -y "-2021"`

### Actual Behaviour

The above example panics. In the real world, you get the following error

```
error: Found argument '-2' which wasn't expected, or isn't valid in this context

If you tried to supply `-2` as a PATTERN use `-- -2`
```

### Expected Behaviour

I think this should run without error giving `"-2021"` as the value of `year_range`

Interestingly, this works when you do:

```rust
use clap::{App, Arg};

fn main() {
    let app =
        App::new(env!("CARGO_PKG_NAME")).arg(Arg::new("year_range").short('y').takes_value(true));
	let _matches = app
            .try_get_matches_from(vec![env!("CARGO_PKG_NAME"), "-y", "\"-2021\""])
            .expect("Parses successfully");
}
```

### Additional Context

I'm trying to accept ranges of years that can be open ended e.g. "2021", "2019-2021", "-2021" (up to 2021), "2019-" (from 2019)

### Debug Output

```
$ cargo run -- -y "-2021"
   Compiling clap_derive v3.0.0-beta.2
   Compiling clap v3.0.0-beta.2
   Compiling clap-bug v0.1.0 (D:\Documents\Git Projects\clap-bug)
    Finished dev [unoptimized + debuginfo] target(s) in 6.68s
     Running `target\debug\clap-bug.exe -y -2021`
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:clap-bug
[            clap::build::app]  App::_derive_display_order:clap-bug
[            clap::build::app]  App::_create_help_and_version
[            clap::build::app]  App::_create_help_and_version: Building --help
[            clap::build::app]  App::_create_help_and_version: Building --version
[clap::build::app::debug_asserts]       App::_debug_asserts
[            clap::build::arg]  Arg::_debug_asserts:year_range
[            clap::build::arg]  Arg::_debug_asserts:help
[            clap::build::arg]  Arg::_debug_asserts:version
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing '"-y"' ([45, 121])
[         clap::parse::parser]  Parser::is_new_arg: "-y":NotFound
[         clap::parse::parser]  Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser]  Parser::is_new_arg: - found
[         clap::parse::parser]  Parser::is_new_arg: starts_new_arg=true
[         clap::parse::parser]  Parser::possible_subcommand: arg="-y"
[         clap::parse::parser]  Parser::get_matches_with: possible_sc=false, sc=None
[         clap::parse::parser]  Parser::parse_short_arg: full_arg="-y"
[         clap::parse::parser]  Parser::parse_short_arg:iter:y
[         clap::parse::parser]  Parser::parse_short_arg:iter:y: Found valid opt or flag
[          clap::util::argstr]  ArgSplit::next: self=ArgSplit { sep: [121, 0, 0, 0], sep_len: 1, val: [121], pos: 0 }
[         clap::parse::parser]  Parser::parse_short_arg:iter:y: i=1, arg_os="y"
[         clap::parse::parser]  Parser::parse_opt; opt=year_range, val=None
[         clap::parse::parser]  Parser::parse_opt; opt.settings=ArgFlags(TAKES_VAL)
[         clap::parse::parser]  Parser::parse_opt; Checking for val...
[         clap::parse::parser]  None
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of: arg=year_range
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of: first instance
[            clap::build::app]  App::groups_for_arg: id=year_range
[         clap::parse::parser]  Parser::parse_opt: More arg vals required...
[         clap::parse::parser]  Parser::get_matches_with: After parse_short_arg Opt(year_range)
[         clap::parse::parser]  Parser::maybe_inc_pos_counter: arg = year_range
[         clap::parse::parser]  Parser::maybe_inc_pos_counter: is it positional?
[         clap::parse::parser]  No
[            clap::build::app]  App::groups_for_arg: id=year_range
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing '"-2021"' ([45, 50, 48, 50, 49])
[         clap::parse::parser]  Parser::is_new_arg: "-2021":Opt(year_range)
[         clap::parse::parser]  Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser]  Parser::is_new_arg: - found
[         clap::parse::parser]  Parser::is_new_arg: starts_new_arg=true
[         clap::parse::parser]  Parser::parse_short_arg: full_arg="-2021"
[         clap::parse::parser]  Parser::parse_short_arg:iter:2
[         clap::output::usage]  Usage::create_usage_with_title
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs=[]
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage]  Usage::needs_flags_tag
[         clap::output::usage]  Usage::needs_flags_tag:iter: f=help
[         clap::output::usage]  Usage::needs_flags_tag:iter: f=version
[         clap::output::usage]  Usage::needs_flags_tag: [FLAGS] not required
[         clap::output::usage]  Usage::create_help_usage: usage=clap-bug.exe [OPTIONS]
[            clap::build::app]  App::color: Color setting...
[            clap::build::app]  Auto
thread 'main' panicked at 'Parses successfully: Error { message: Colorizer { use_stderr: true, color_when: Auto, pieces: [("error:", Some(Red)), (" ", None), ("Found argument '", None), ("-2", Some(Yellow)), ("' which wasn't expected, or isn't valid in this context", None), ("\n\nIf you tried to supply `-2` as a PATTERN use `-- -2`", None), ("\n\n", None), ("USAGE:\n    clap-bug.exe [OPTIONS]", None), ("\n\nFor more information try ", None), ("--help", Some(Green)), ("\n", None)] }, kind: UnknownArgument, info: ["-2"] }', src\main.rs:11:10
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
error: process didn't exit successfully: `target\debug\clap-bug.exe -y -2021` (exit code: 101)
```

---

_Label `T: bug` added by @alpha-tango-kilo on 2021-08-02 15:51_

---

_Comment by @ldm0 on 2021-08-02 15:55_

Have you tried [`AllowLeadingHyphen`](https://docs.rs/clap/2.33.3/clap/enum.AppSettings.html#variant.AllowLeadingHyphen)?

---

_Locked by @clap-rs on 2021-08-02 15:56_

---

_Closed by @pksunkara on 2021-08-02 15:56_

---
