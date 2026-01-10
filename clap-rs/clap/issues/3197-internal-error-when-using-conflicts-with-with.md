---
number: 3197
title: "Internal error when using `conflicts_with` with `ArgGroup`"
type: issue
state: closed
author: elomatreb
labels:
  - C-bug
  - E-medium
  - A-validators
assignees: []
created_at: 2021-12-17T16:38:24Z
updated_at: 2021-12-23T20:30:10Z
url: https://github.com/clap-rs/clap/issues/3197
synced_at: 2026-01-10T01:27:35Z
---

# Internal error when using `conflicts_with` with `ArgGroup`

---

_Issue opened by @elomatreb on 2021-12-17 16:38_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.57.0 (f1edd0429 2021-11-29)

### Clap Version

3.0.0-rc.7

### Minimal reproducible code

```rust
use clap::{App, Arg, ArgGroup};

fn main() {
    let m = App::new("demoapp")
        .arg(Arg::new("foo").long("foo").conflicts_with("argument-group"))
        .arg(Arg::new("bar").long("bar"))
        .group(ArgGroup::new("argument-group").arg("bar"))
        .get_matches();

    dbg!(m);
}

```


### Steps to reproduce the bug with the above code

`cargo run -- --foo`

### Actual Behaviour

The program panics, with the following internal error:

```
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/clap-rs/clap/issues', /home/elomatreb/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-rc.7/src/parse/validator.rs:240:18
stack backtrace:
   0: rust_begin_unwind
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/std/src/panicking.rs:517:5
   1: core::panicking::panic_fmt
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/core/src/panicking.rs:100:14
   2: core::panicking::panic_display
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/core/src/panicking.rs:64:5
   3: core::option::expect_failed
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/core/src/option.rs:1638:5
   4: core::option::Option<T>::expect
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/core/src/option.rs:709:21
   5: clap::parse::validator::Validator::build_conflict_err
             at /home/elomatreb/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-rc.7/src/parse/validator.rs:237:25
   6: clap::parse::validator::Validator::validate_conflicts::{{closure}}
             at /home/elomatreb/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-rc.7/src/parse/validator.rs:304:33
   7: core::iter::traits::iterator::Iterator::try_for_each::call::{{closure}}
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/core/src/iter/traits/iterator.rs:2053:26
   8: core::iter::adapters::filter::filter_try_fold::{{closure}}
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/core/src/iter/adapters/filter.rs:44:44
   9: core::iter::adapters::map::map_try_fold::{{closure}}
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/core/src/iter/adapters/map.rs:91:21
  10: core::iter::traits::iterator::Iterator::try_fold
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/core/src/iter/traits/iterator.rs:1995:21
  11: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::try_fold
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/core/src/iter/adapters/map.rs:117:9
  12: <core::iter::adapters::filter::Filter<I,P> as core::iter::traits::iterator::Iterator>::try_fold
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/core/src/iter/adapters/filter.rs:93:9
  13: core::iter::traits::iterator::Iterator::try_for_each
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/core/src/iter/traits/iterator.rs:2056:9
  14: clap::parse::validator::Validator::validate_conflicts
             at /home/elomatreb/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-rc.7/src/parse/validator.rs:264:9
  15: clap::parse::validator::Validator::validate
             at /home/elomatreb/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-rc.7/src/parse/validator.rs:72:9
  16: clap::parse::parser::Parser::get_matches_with
             at /home/elomatreb/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-rc.7/src/parse/parser.rs:702:9
  17: clap::build::app::App::_do_parse
             at /home/elomatreb/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-rc.7/src/build/app/mod.rs:2565:29
  18: clap::build::app::App::try_get_matches_from_mut
             at /home/elomatreb/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-rc.7/src/build/app/mod.rs:649:9
  19: clap::build::app::App::get_matches_from
             at /home/elomatreb/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-rc.7/src/build/app/mod.rs:519:9
  20: clap::build::app::App::get_matches
             at /home/elomatreb/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-rc.7/src/build/app/mod.rs:431:9
  21: claptest::main
             at ./src/main.rs:4:13
  22: core::ops::function::FnOnce::call_once
             at /rustc/f1edd0429582dd29cccacaf50fd134b05593bd9c/library/core/src/ops/function.rs:227:5
```

### Expected Behaviour

The program should work, outputting something like
```
[src/main.rs:10] m = ArgMatches {
    valid_args: [
        help,
        foo,
        bar,
        argument-group,
    ],
    valid_subcommands: [],
    args: {
        foo: MatchedArg {
            occurs: 1,
            ty: CommandLine,
            indices: [
                1,
            ],
            vals: [],
            ignore_case: false,
            invalid_utf8_allowed: Some(
                false,
            ),
        },
    },
    subcommand: None,
}
```

### Additional Context

I am trying to make a single argument conflict with a `ArgGroup` (to save on repetition). If I instead place the `conflicts_with("foo")` on  the argument group definition, it works as expected.

I'm assuming using the `conflicts_with` method on the individual argument should be possible too, but if it isn't supposed to be possible, a better error message would be good.

### Debug Output

```
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:demoapp
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_check_help_and_version: Removing generated version
[            clap::build::app] 	App::_propagate_global_args:demoapp
[            clap::build::app] 	App::_derive_display_order:demoapp
[clap::build::app::debug_asserts] 	App::_debug_asserts
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:help
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:foo
[clap::build::arg::debug_asserts] 	Arg::_debug_asserts:bar
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--foo")' ([45, 45, 102, 111, 111])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("--foo")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::parse_long_arg
[         clap::parse::parser] 	Parser::parse_long_arg: cur_idx:=1
[         clap::parse::parser] 	Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser] 	No
[         clap::parse::parser] 	Parser::parse_long_arg: Found valid opt or flag '--foo'
[         clap::parse::parser] 	Parser::check_for_help_and_version_str
[         clap::parse::parser] 	Parser::check_for_help_and_version_str: Checking if --RawOsStr("foo") is help or version...
[         clap::parse::parser] 	Neither
[         clap::parse::parser] 	Parser::parse_long_arg: Presence validated
[         clap::parse::parser] 	Parser::parse_flag
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_arg: id=foo
[            clap::build::app] 	App::groups_for_arg: id=foo
[         clap::parse::parser] 	Parser::get_matches_with: After parse_long_arg ValuesDone
[         clap::parse::parser] 	Parser::remove_overrides
[         clap::parse::parser] 	Parser::remove_overrides:iter:foo
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::validate_exclusive:iter:foo
[      clap::parse::validator] 	Validator::gather_conflicts
[      clap::parse::validator] 	Validator::gather_conflicts:iter: id=foo
[            clap::build::app] 	App::groups_for_arg: id=foo
[      clap::parse::validator] 	Validator::validate_conflicts:iter:argument-group
[            clap::build::app] 	App::unroll_args_in_group: group=argument-group
[            clap::build::app] 	App::unroll_args_in_group:iter: entity=bar
[            clap::build::app] 	App::unroll_args_in_group:iter: this is an arg
[      clap::parse::validator] 	Validator::build_conflict_err: name=argument-group
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage] 	Usage::needs_options_tag
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=help
[         clap::output::usage] 	Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=foo
[            clap::build::app] 	App::groups_for_arg: id=foo
[         clap::output::usage] 	Usage::needs_options_tag:iter: [OPTIONS] required
[         clap::output::usage] 	Usage::create_help_usage: usage=claptest [OPTIONS]
[            clap::build::app] 	App::unroll_args_in_group: group=argument-group
[            clap::build::app] 	App::unroll_args_in_group:iter: entity=bar
[            clap::build::app] 	App::unroll_args_in_group:iter: this is an arg
```

---

_Label `C-bug` added by @elomatreb on 2021-12-17 16:38_

---

_Comment by @epage on 2021-12-17 17:02_

Backported this to clap2 and it works
```rust
use clap::{App, Arg, ArgGroup};

fn main() {
    let m = App::new("demoapp")
        .arg(
            Arg::with_name("foo")
                .long("foo")
                .conflicts_with("argument-group"),
        )
        .arg(Arg::with_name("bar").long("bar"))
        .group(ArgGroup::with_name("argument-group").arg("bar"))
        .get_matches();

    dbg!(m);
}
```

Its interesting the panic in clap3 happens with `cargo run -- --foo` but not `--bar`, `--foo --bar`, or `--help`.

Debug output:
```
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_blacklist:iter:foo;
DEBUG:clap:Parser::groups_for_arg: name=foo
DEBUG:clap:Parser::groups_for_arg: Searching through groups...
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:foo: vals=[]
DEBUG:clap:Validator::validate_arg_requires:foo;
DEBUG:clap:Validator::validate_arg_num_occurs: a=foo;
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=foo;
DEBUG:clap:Parser::groups_for_arg: name=foo
DEBUG:clap:Parser::groups_for_arg: Searching through groups...
DEBUG:clap:usage::needs_flags_tag:iter: [FLAGS] required
DEBUG:clap:usage::create_help_usage: usage=test-clap [FLAGS]
DEBUG:clap:ArgMatcher::get_global_values: global_arg_vec=[]
[src/main.rs:14] m = ArgMatches {
    args: {
        "foo": MatchedArg {
            occurs: 1,
            indices: [
                1,
            ],
            vals: [],
        },
    },
    subcommand: None,
    usage: Some(
        "USAGE:\n    test-clap [FLAGS]",
    ),
}
```

---

_Added to milestone `3.0` by @epage on 2021-12-17 17:02_

---

_Label `A-validators` added by @epage on 2021-12-17 17:26_

---

_Label `E-medium` added by @epage on 2021-12-17 17:26_

---

_Referenced in [clap-rs/clap#2512](../../clap-rs/clap/pulls/2512.md) on 2021-12-21 21:46_

---

_Comment by @epage on 2021-12-21 21:51_

We handle conflicts in two phases
- gather potential conflicts: we unconditionally add `argument-group` when `--foo` is present
- check conflicts: with `--foo` present and `argument-group` previously added, these conflict (`arg_conf_with_gr`)

There is some deleted code in `gather_conflicts` so I wondered if #2512 is what caused this but I went to 6e158d9f8~ and it still paniced.

---

_Referenced in [clap-rs/clap#3205](../../clap-rs/clap/pulls/3205.md) on 2021-12-22 02:41_

---

_Referenced in [clap-rs/clap#3206](../../clap-rs/clap/pulls/3206.md) on 2021-12-22 02:47_

---

_Comment by @epage on 2021-12-22 17:33_

The crux of the issue is how we communicate between the two phases (gather, check): we gather groups for both present args **and** that conflict with present args.  These become contradictory.

What I'm trying to understand next is why the algorithm shifted so much from [clap v2](https://github.com/clap-rs/clap/blob/v2-master/src/app/validator.rs#L193)

---

_Comment by @epage on 2021-12-22 17:58_

7673dfc0 appears to be the first major change though at a glance it seems like its keeping the basic operations, just changing the code to show errors prioritized by argument index.

5a06a827 is what seems to fundamentally change things to address #1158.

---

_Referenced in [clap-rs/clap#3212](../../clap-rs/clap/pulls/3212.md) on 2021-12-23 19:52_

---

_Closed by @epage on 2021-12-23 20:18_

---

_Comment by @epage on 2021-12-23 20:30_

3.0.0-rc.8 is released with the fix for this

---
