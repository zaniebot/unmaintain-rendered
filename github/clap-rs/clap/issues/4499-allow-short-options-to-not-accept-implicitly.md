---
number: 4499
title: "Allow short options to not accept implicitly attached values (`-sfoo`)"
type: issue
state: open
author: zohnannor
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2022-11-21T20:44:47Z
updated_at: 2023-02-14T22:44:30Z
url: https://github.com/clap-rs/clap/issues/4499
synced_at: 2026-01-07T13:12:20-06:00
---

# Allow short options to not accept implicitly attached values (`-sfoo`)

---

_Issue opened by @zohnannor on 2022-11-21 20:44_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.65.0 (897e37553 2022-11-02)

### Clap Version

4.0.26

### Minimal reproducible code

```rust
/// Slightly modified pacman example: https://github.com/clap-rs/clap/blob/f5dcfc5bd92ff5bd52c1d74f202d2565e0f3102f/examples/pacman.rs

use clap::{Arg, ArgAction, Command};

fn main() {
    let matches = Command::new("pacman")
        .subcommand(
            Command::new("query")
                .short_flag('Q')
                .long_flag("query")
                .arg(
                    Arg::new("search")
                        .short('s')
                        .long("search")
                        .conflicts_with("info")
                        .action(ArgAction::Set)
                        .num_args(1..),
                )
                .arg(
                    Arg::new("info")
                        .long("info")
                        .short('i')
                        .conflicts_with("search")
                        .action(ArgAction::Set)
                        .num_args(1..),
                ),
        )
        .subcommand(
            Command::new("sync")
                .short_flag('S')
                .long_flag("sync")
                .arg(
                    Arg::new("search")
                        .short('s')
                        .long("search")
                        .conflicts_with("info")
                        .action(ArgAction::Set)
                        .num_args(1..),
                )
                .arg(
                    Arg::new("info")
                        .long("info")
                        .conflicts_with("search")
                        .short('i')
                        .action(ArgAction::SetTrue),
                ),
        )
        .get_matches();

    dbg!(matches.subcommand());
}
```


### Steps to reproduce the bug with the above code

`cargo r --example pacman -- -Ssi`

### Actual Behaviour

The above command arguments (`-Ssi`) are interpreted as `-S -s i`, the `s` flag takes `i` as an argument.

### Expected Behaviour

This behavior did confuse me honestly. I expected same output as from `-Sis test`:
```
error: The argument '--info' cannot be used with '--search <search>...'

Usage: pacman {sync|--sync|-S} --info

For more information try '--help'
```

(To be fair, I also expected `-Sis` to fail, but it errors with `error: The argument '--search <search>...' requires a value but none was supplied`, I think it should *first* check for conflicts and then try to parse flag's params.)


### Additional Context

Command with these args | Works? | Should? | Notes
:- | :-: | :-: | :-
`-Si` | Yes | Yes :white_check_mark:| 
`-Ss` | No | No :white_check_mark:| Errors with `The argument '--search <search>...' requires a value but none was supplied`
`-Sis` | No | No :white_check_mark: | Errors with `The argument '--search <search>...' requires a value but none was supplied`
`-Ssi` | Yes | No :x:| What this issue is about
`-Sis test` | No | No :white_check_mark:| Errors with `The argument '--info' cannot be used with '--search <search>...'`
`-Ssi test` | No | No :grey_question: | But, here it errors with `Found argument 'test' which wasn't expected, or isn't valid in this context`, but *really* it should say `The argument '--info' cannot be used with '--search <search>...'`
`-S -i` | Yes | Yes :white_check_mark:| 
`-S -s` | No | No :white_check_mark:| Errors with `The argument '--search <search>...' requires a value but none was supplied`
`-S -i -s` | No | No :white_check_mark: | Errors with `The argument '--search <search>...' requires a value but none was supplied`
`-S -s -i` | No | No ~:white_check_mark:~ :grey_question: | Errors with `The argument '--search <search>...' requires a value but none was supplied`, ~which should happen with `-Ssi` too~, it should error with `The argument '--info' cannot be used with '--search <search>...'`

I apologize for possibly incorrect terminology in the issue title.

### Debug Output

```
cargo r --features debug --example pacman -- -Ssi test
   Compiling adler v1.0.2
   Compiling gimli v0.26.2
   Compiling object v0.29.0
   Compiling rustc-demangle v0.1.21
   Compiling miniz_oxide v0.5.3
   Compiling addr2line v0.17.0
   Compiling backtrace v0.3.66
   Compiling clap v4.0.26 (/home/zohnannor/clap)
    Finished dev [unoptimized + debuginfo] target(s) in 11.34s
     Running `target/debug/examples/pacman -Ssi test`
[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="pacman"
[      clap::builder::command] 	Command::_propagate:pacman
[      clap::builder::command] 	Command::_check_help_and_version:pacman expand_help_tree=false
[      clap::builder::command] 	Command::long_help_exists
[      clap::builder::command] 	Command::_check_help_and_version: Building default --help
[      clap::builder::command] 	Command::_check_help_and_version: Building help subcommand
[      clap::builder::command] 	Command::_propagate_global_args:pacman
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("-Ssi")' ([45, 83, 115, 105])
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("-Ssi")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("Ssi"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['S', 's', 'i']) }, invalid_suffix: None }
[        clap::parser::parser] 	Parser::parse_short_arg:iter:S
[        clap::parser::parser] 	Parser::parse_short_arg:iter:S: subcommand=sync
[        clap::parser::parser] 	Parser::parse_short_arg: cur_idx:=1
[        clap::parser::parser] 	Parser::get_matches_with: After parse_short_arg FlagSubCommand("sync")
[        clap::parser::parser] 	Parser::get_matches_with:FlagSubCommandShort: subcmd_name=sync, keep_state=true, flag_subcmd_skip=1
[        clap::parser::parser] 	Parser::parse_subcommand
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs=[]
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[      clap::builder::command] 	Command::_build_subcommand Setting bin_name of sync to "pacman sync"
[      clap::builder::command] 	Command::_build_subcommand Setting display_name of sync to "pacman-sync"
[      clap::builder::command] 	Command::_build: name="sync"
[      clap::builder::command] 	Command::_propagate:sync
[      clap::builder::command] 	Command::_check_help_and_version:sync expand_help_tree=false
[      clap::builder::command] 	Command::long_help_exists
[      clap::builder::command] 	Command::_check_help_and_version: Building default --help
[      clap::builder::command] 	Command::_propagate_global_args:sync
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:search
[clap::builder::debug_asserts] 	Arg::_debug_asserts:info
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::parse_subcommand: About to parse sc=sync
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("-Ssi")' ([45, 83, 115, 105])
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("-Ssi")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("Ssi"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['S', 's', 'i']) }, invalid_suffix: None }
[        clap::parser::parser] 	Parser::parse_short_arg:iter:s
[        clap::parser::parser] 	Parser::parse_short_arg:iter:s: Found valid opt or flag
[        clap::parser::parser] 	Parser::parse_short_arg:iter:s: val=RawOsStr("i") (bytes), val=[105] (ascii), short_arg=ShortFlags { inner: RawOsStr("Ssi"), utf8_prefix: CharIndices { front_offset: 2, iter: Chars(['i']) }, invalid_suffix: None }
[        clap::parser::parser] 	Parser::parse_opt_value; arg=search, val=Some(RawOsStr("i")), has_eq=false
[        clap::parser::parser] 	Parser::parse_opt_value; arg.settings=ArgFlags(NO_OP)
[        clap::parser::parser] 	Parser::parse_opt_value; Checking for val...
[        clap::parser::parser] 	Parser::react action=Set, identifier=Some(Short), source=CommandLine
[        clap::parser::parser] 	Parser::react: cur_idx:=2
[        clap::parser::parser] 	Parser::remove_overrides: id="search"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="search", source=CommandLine
[      clap::builder::command] 	Command::groups_for_arg: id="search"
[        clap::parser::parser] 	Parser::push_arg_values: ["i"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=3
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=search, pending=0
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: expected=1..=18446744073709551615, actual=0
[        clap::parser::parser] 	Parser::react not enough values passed in, leaving it to the validator to complain
[        clap::parser::parser] 	Parser::get_matches_with: After parse_short_arg ValuesDone
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("test")' ([116, 101, 115, 116])
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("test")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::get_matches_with: Positional counter...1
[        clap::parser::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::needs_options_tag
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=search
[      clap::builder::command] 	Command::groups_for_arg: id="search"
[         clap::output::usage] 	Usage::needs_options_tag:iter: [OPTIONS] required
[         clap::output::usage] 	Usage::get_args: incls=[]
[         clap::output::usage] 	Usage::get_args: unrolled_reqs=[]
[         clap::output::usage] 	Usage::get_args: ret_val=[]
[         clap::output::usage] 	Usage::create_help_usage: usage=pacman {sync|--sync|-S} [OPTIONS]
[      clap::builder::command] 	Command::color: Color setting...
[      clap::builder::command] 	Auto
[      clap::builder::command] 	Command::color: Color setting...
[      clap::builder::command] 	Auto
error: Found argument 'test' which wasn't expected, or isn't valid in this context

Usage: pacman {sync|--sync|-S} [OPTIONS]

For more information try '--help'
```

---

_Label `C-bug` added by @zohnannor on 2022-11-21 20:44_

---

_Comment by @epage on 2022-11-22 00:29_

> The above command arguments (`-Ssi`) are interpreted as `-S -s i`, the s flag takes i as an argument.

`-s` takes a value and short flags are allowed to have an implicitly attached value, so this is expected.  I think you can set `requires_equals` to bypass the implicitly attached value but that will require an `=` always and not just disable implicitly attached values.

---

_Comment by @zohnannor on 2022-11-22 16:53_

Yes, it is working as it is written in the code, as expected. What I'm saying is (I guess, https://github.com/clap-rs/clap/labels/C-bug label is incorrect here, sorry), shouldn't there be **at least** an option to forbid attached values for arguments? Also, regarding `-S -s -i`, shouldn't it check for conflicts first? `pacman` itself works like that:
```
$ sudo pacman -Sis
error: invalid option: '--info' and '--search' may not be used together
$ sudo pacman -Ssi
error: invalid option: '--info' and '--search' may not be used together
$ sudo pacman -S -s -i
error: invalid option: '--info' and '--search' may not be used together
```

---

_Comment by @epage on 2022-11-22 18:07_

> shouldn't there be at least an option to forbid attached values for arguments?

Unsure, will have to give it some thought.  It seems like some more flexibility around implicitly attached, explicitly attached, and detached values could help with some cases.  See also #3030

---

_Label `C-bug` removed by @epage on 2022-11-22 18:08_

---

_Label `C-enhancement` added by @epage on 2022-11-22 18:08_

---

_Label `A-parsing` added by @epage on 2022-11-22 18:08_

---

_Label `S-waiting-on-design` added by @epage on 2022-11-22 18:08_

---

_Renamed from "Flag takes next flag as value (pacman example)" to "Allow short options to not accept implicitly attached values (`-sfoo`)" by @epage on 2022-11-22 18:08_

---

_Comment by @epage on 2023-02-13 14:27_

btw by disallowing attached values, you might not get
```
error: The argument '--info' cannot be used with '--search <search>...'

Usage: pacman {sync|--sync|-S} --info

For more information try '--help'
```

but instead an error about `--search` missing a value

---

_Referenced in [clap-rs/clap#4702](../../clap-rs/clap/issues/4702.md) on 2023-02-13 16:03_

---

_Comment by @zohnannor on 2023-02-14 22:44_

Yes, and I've also mentioned that in the table for entry `-S -s -i`, it is counter-intuitive and possibly even deserves its own issue. I presume it depends on the order in which `clap` parses arguments and it would be a breaking change to modify it.

---
