```yaml
number: 3330
title: Conflicting args both present
type: issue
state: closed
author: nissaofthesea
labels:
  - C-bug
  - E-medium
  - A-validators
assignees: []
created_at: 2022-01-22T23:19:37Z
updated_at: 2022-01-24T16:55:50Z
url: https://github.com/clap-rs/clap/issues/3330
synced_at: 2026-01-12T16:14:14Z
```

# Conflicting args both present

---

_@nissaofthesea_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.58.1 (db9d1b20b 2022-01-20)

### Clap Version

3.0.10

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Debug, Parser)]
struct Args {
    /// Simulate instead of using USB
    #[clap(short = 's', conflicts_with = "usb")]
    sim: bool,

    /// Sets the USB device to use
    #[clap(
        short = 'd',
        required_unless_present = "sim",
        group = "usb"
    )]
    device: Option<u32>,

    /// Sets the time in milliseconds to wait for USB reads and writes
    #[clap(
        short = 't',
        default_value_t = 1000,
        required_unless_present = "sim",
        group = "usb"
    )]
    timeout: u64,
}

fn main() {
    dbg!(Args::parse());
}
```

In `Cargo.toml`, the "derive" and "debug" features are enabled.


### Steps to reproduce the bug with the above code

```console
$ cargo run -- -s -d 123 # arbitrary number
<output from clap "debug" feature is omitted>
[src/main.rs:28] Args::parse() = Args {
    sim: true,
    device: Some(
        123,
    ),
    timeout: 1000,
}
```

### Actual Behaviour

The `sim` option conflicts with the group `usb` (members: `device`, `timeout`). But when `sim` is present alongside `device`, clap doesn't give any errors. In the output of the program, you can see `sim` is true, and `device` has some value.

### Expected Behaviour

I expect that `sim` and `device` both being present is an error, since `sim` conflicts with the group that `device` is a member of.

### Additional Context

Interestingly, `sim` and `timeout` both present gives an error as expected. Also, if `timeout` is changed to not have a default value, everything works as expected.

Lastly, explicitly listing conflicting args using `conflicts_with_all` works.

### Debug Output

```console
$ cargo run -- -s -d 123
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:reproducable-example
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Removing generated version
[            clap::build::app] 	App::_propagate_global_args:reproducable-example
[            clap::build::app] 	App::_derive_display_order:reproducable-example
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:sim
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:device
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:timeout
[clap::build::app::debug_asserts] 	App::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("-s")' ([45, 115])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("-s")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::parse_short_arg: short_arg=RawOsStr("s")
[         clap::parse::parser] 	Parser::parse_short_arg:iter:s
[         clap::parse::parser] 	Parser::parse_short_arg:iter:s: cur_idx:=1
[         clap::parse::parser] 	Parser::parse_short_arg:iter:s: Found valid opt or flag
[         clap::parse::parser] 	Parser::check_for_help_and_version_char
[         clap::parse::parser] 	Parser::check_for_help_and_version_char: Checking if -s is help or version...
[         clap::parse::parser] 	Neither
[         clap::parse::parser] 	Parser::parse_flag
[         clap::parse::parser] 	Parser::remove_overrides: id=sim
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_arg: id=sim
[            clap::build::app] 	App::groups_for_arg: id=sim
[         clap::parse::parser] 	Parser::get_matches_with: After parse_short_arg ValuesDone
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("-d")' ([45, 100])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("-d")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::parse_short_arg: short_arg=RawOsStr("d")
[         clap::parse::parser] 	Parser::parse_short_arg:iter:d
[         clap::parse::parser] 	Parser::parse_short_arg:iter:d: cur_idx:=2
[         clap::parse::parser] 	Parser::parse_short_arg:iter:d: Found valid opt or flag
[         clap::parse::parser] 	Parser::parse_short_arg:iter:d: val=RawOsStr("") (bytes), val=[] (ascii), short_arg=RawOsStr("d")
[         clap::parse::parser] 	Parser::parse_opt; opt=device, val=None
[         clap::parse::parser] 	Parser::parse_opt; opt.settings=ArgFlags(TAKES_VAL)
[         clap::parse::parser] 	Parser::parse_opt; Checking for val...
[         clap::parse::parser] 	Parser::parse_opt: More arg vals required...
[         clap::parse::parser] 	Parser::remove_overrides: id=device
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_arg: id=device
[            clap::build::app] 	App::groups_for_arg: id=device
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_group: id=usb
[            clap::build::app] 	App::groups_for_arg: id=device
[         clap::parse::parser] 	Parser::get_matches_with: After parse_short_arg Opt(device)
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("123")' ([49, 50, 51])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=device, val=RawOsStr("123")
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."123"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=3
[            clap::build::app] 	App::groups_for_arg: id=device
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=device
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:device:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:device: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:device: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:timeout:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:timeout: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:timeout: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=timeout
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."1000"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=4
[            clap::build::app] 	App::groups_for_arg: id=timeout
[         clap::parse::parser] 	Parser::add_value:iter:timeout: doesn't have default missing vals
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::validate_exclusive:iter:sim
[      clap::parse::validator] 	Validator::validate_exclusive:iter:device
[      clap::parse::validator] 	Validator::validate_exclusive:iter:usb
[      clap::parse::validator] 	Validator::validate_exclusive:iter:timeout
[      clap::parse::validator] 	Validator::validate_conflicts::iter: id=sim
[      clap::parse::validator] 	Conflicts::gather_conflicts
[            clap::build::app] 	App::groups_for_arg: id=sim
[      clap::parse::validator] 	Conflicts::gather_direct_conflicts id=sim, conflicts=[usb]
[            clap::build::app] 	App::groups_for_arg: id=device
[      clap::parse::validator] 	Conflicts::gather_direct_conflicts id=device, conflicts=[timeout]
[      clap::parse::validator] 	Validator::validate_conflicts::iter: id=device
[      clap::parse::validator] 	Conflicts::gather_conflicts
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::gather_requirements:iter:sim
[      clap::parse::validator] 	Validator::gather_requirements:iter:device
[      clap::parse::validator] 	Validator::gather_requirements:iter:usb
[      clap::parse::validator] 	Validator::gather_requirements:iter:usb:group
[      clap::parse::validator] 	Validator::gather_requirements:iter:timeout
[      clap::parse::validator] 	Validator::validate_required_unless
[      clap::parse::validator] 	Validator::validate_matched_args
[      clap::parse::validator] 	Validator::validate_matched_args:iter:sim: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="sim"
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "sim"=1
[      clap::parse::validator] 	Validator::validate_matched_args:iter:device: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "123",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="device"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "device"=1
[      clap::parse::validator] 	Validator::validate_matched_args:iter:usb: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "123",
                        ],
                        [
                            "1000",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_matched_args:iter:timeout: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "1000",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="timeout"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "timeout"=0
[    clap::parse::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[help]
<output from actual program is omitted>

---

_Label `C-bug` added by @nissaofthesea on 2022-01-22 23:19_

---

_Comment by @epage on 2022-01-23 01:32_

So not dug into the debug output (which should have some useful stuff because I had to debug and rewrite conflicts right before release) or the code but my guess is that the core of the issue is using a flag.

For an arg, we track presence, occurrences, and values.  A lot of these rules only work on a value but flags never set a value and rely purely on presence.

This and several related issues has been building in me a sense that special casing flags like this is a mistake and that we should instead define an action that can be performed when a flag is present, like setting a value to true, counting, etc.  Maybe I'm just biased from my CLI background being heavily in Python and this is how `argparse` does it.

---

_Label `A-validators` added by @epage on 2022-01-23 01:32_

---

_Label `S-triage` added by @epage on 2022-01-23 01:32_

---

_Comment by @mzagrabe on 2022-01-24 14:51_

On Sat, Jan 22, 2022 at 7:32 PM Ed Page ***@***.***> wrote:

> So not dug into the debug output (which should have some useful stuff
> because I had to debug and rewrite conflicts right before release) or the
> code but my guess is that the core of the issue is using a flag.
>
> For an arg, we track presence, occurrences, and values. A lot of these
> rules only work on a value but flags never set a value and rely purely on
> presence.
>
> This and several related issues has been building in me a sense that
> special casing flags like this is a mistake and that we should instead
> define an action that can be performed when a flag is present, like setting
> a value to true, counting, etc. Maybe I'm just biased from my CLI
> background being heavily in Python and this is how argparse does it.
>
>
I think imitating argparse is sensible. Not only does it give a general
roadmap for decisions making when encountering design considerations, but
also provides familiarity and reduces friction for those of us with arparse
backgrounds now relying on clap.

-m

> Message ID: ***@***.***>
>


---

_Referenced in [pwinckles/rocfl#18](../../pwinckles/rocfl/issues/18.md) on 2022-01-24 15:02_

---

_Comment by @epage on 2022-01-24 16:21_

Ok, looks like this is unrelated to that problem.

We are incorrectly handling defaults in groups.  We track the source of a group (command-line, env, defaults).  Unfortunately, defaults are overriding any prior source.  So instead of treating the `usb` group as being from the command line, we are treating it as being from defaults (because they are the last one to write the source) and so there is no conflict (we don't conflict with defaults).

A workaround until this is fixed is to not use clap-defined defaults.

---

_Label `S-triage` removed by @epage on 2022-01-24 16:22_

---

_Label `E-medium` added by @epage on 2022-01-24 16:22_

---

_Comment by @pwinckles on 2022-01-24 16:26_

> Ok, looks like this is unrelated to that problem.

Whoops, sorry. I clearly only skimmed this issue and did not read. :blush: I'll create a new issue for the problem I'm seeing.


---

_Referenced in [clap-rs/clap#3334](../../clap-rs/clap/pulls/3334.md) on 2022-01-24 16:36_

---

_Closed by @epage on 2022-01-24 16:51_

---

_Comment by @epage on 2022-01-24 16:55_

v3.0.11 is released with the fix

---

_Referenced in [clap-rs/clap#3337](../../clap-rs/clap/issues/3337.md) on 2022-01-25 06:22_

---

_Referenced in [uutils/coreutils#3262](../../uutils/coreutils/issues/3262.md) on 2022-03-17 10:10_

---
