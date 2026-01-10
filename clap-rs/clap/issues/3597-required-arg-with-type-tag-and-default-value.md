---
number: 3597
title: "Required arg with <TYPE> tag and default value always errors in debug build but never in release"
type: issue
state: closed
author: nbvdkamp
labels:
  - C-bug
assignees: []
created_at: 2022-03-31T21:45:42Z
updated_at: 2022-04-01T12:16:26Z
url: https://github.com/clap-rs/clap/issues/3597
synced_at: 2026-01-10T01:27:44Z
---

# Required arg with <TYPE> tag and default value always errors in debug build but never in release

---

_Issue opened by @nbvdkamp on 2022-03-31 21:45_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.59.0 (9d1b2106e 2022-02-23)

### Clap Version

3.1.6

### Minimal reproducible code

```rust
use clap::{arg, Command};

fn main() {
    let matches = Command::new("")
            .arg(arg!(--foo <FOO>)
                    .default_value("default")
            ).get_matches();
    
    println!("{}", matches.value_of("foo").unwrap());
}
```


### Steps to reproduce the bug with the above code

These 
`cargo run -- --foo bar`
`cargo run --release -- --foo bar`
give differing results

### Actual Behaviour

Running a debug build `cargo run -- --foo bar` or  `cargo run` results in:
`thread 'main' panicked at 'Argument 'foo' is required and can't have a default value'`

But running a release build with the flag set `cargo run --release -- --foo bar` results in:
`bar`
And leaving the flag out `cargo run --release` results in:
`default`

### Expected Behaviour

When running with the flag it should never error. When leaving out the flag I am not certain what the correct behaviour is but it should be consistent between debug and release builds.

### Additional Context

_No response_

### Debug Output

For `cargo run -- --foo bar`:
```
[        clap::build::command]  App::_do_parse
[        clap::build::command]  App::_build
[        clap::build::command]  App::_propagate:
[        clap::build::command]  App::_check_help_and_version:
[        clap::build::command]  App::_check_help_and_version: Removing generated version
[        clap::build::command]  App::_propagate_global_args:
[        clap::build::command]  App::_derive_display_order:
[  clap::build::debug_asserts]  Command::_debug_asserts
[  clap::build::debug_asserts]  Arg::_debug_asserts:help
[  clap::build::debug_asserts]  Arg::_debug_asserts:foo
thread 'main' panicked at 'Argument 'foo' is required and can't have a default value'
```

For `cargo run --release -- --foo bar`:
```
[        clap::build::command]  App::_do_parse
[        clap::build::command]  App::_build
[        clap::build::command]  App::_propagate:
[        clap::build::command]  App::_check_help_and_version:
[        clap::build::command]  App::_check_help_and_version: Removing generated version
[        clap::build::command]  App::_propagate_global_args:
[        clap::build::command]  App::_derive_display_order:
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsString("--foo")' ([45, 45, 102, 111, 111])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=RawOsStr("--foo")
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::parse_long_arg
[         clap::parse::parser]  Parser::parse_long_arg: cur_idx:=1
[         clap::parse::parser]  Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser]  No
[         clap::parse::parser]  Parser::parse_long_arg: Found valid opt or flag '--foo <FOO>'
[         clap::parse::parser]  Parser::parse_long_arg: Found an opt with value 'None'
[         clap::parse::parser]  Parser::parse_opt; opt=foo, val=None
[         clap::parse::parser]  Parser::parse_opt; opt.settings=ArgFlags(REQUIRED | TAKES_VAL)
[         clap::parse::parser]  Parser::parse_opt; Checking for val...
[         clap::parse::parser]  Parser::parse_opt: More arg vals required...
[         clap::parse::parser]  Parser::remove_overrides: id=[hash: 41CB8A11A30AC4B8]
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=[hash: 41CB8A11A30AC4B8]
[        clap::build::command]  App::groups_for_arg: id=[hash: 41CB8A11A30AC4B8]
[        clap::build::command]  App::groups_for_arg: id=[hash: 41CB8A11A30AC4B8]
[         clap::parse::parser]  Parser::get_matches_with: After parse_long_arg Opt([hash: 41CB8A11A30AC4B8])
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsString("bar")' ([98, 97, 114])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::add_val_to_arg; arg=foo, val=RawOsStr("bar")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."bar"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=2
[        clap::build::command]  App::groups_for_arg: id=[hash: 41CB8A11A30AC4B8]
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=foo
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_defaults
[         clap::parse::parser]  Parser::add_defaults:iter:foo:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:foo: has default vals
[         clap::parse::parser]  Parser::add_value:iter:foo: was used
[         clap::parse::parser]  Parser::add_value:iter:foo: doesn't have default missing vals
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::validate_exclusive:iter:[hash: 41CB8A11A30AC4B8]
[      clap::parse::validator]  Validator::validate_conflicts::iter: id=[hash: 41CB8A11A30AC4B8]
[      clap::parse::validator]  Conflicts::gather_conflicts
[      clap::parse::validator]  Validator::validate_required: required=ChildGraph([Child { id: [hash: 41CB8A11A30AC4B8], children: [] }])
[      clap::parse::validator]  Validator::gather_requires
[      clap::parse::validator]  Validator::gather_requires:iter:[hash: 41CB8A11A30AC4B8]
[      clap::parse::validator]  Validator::validate_required_unless
[      clap::parse::validator]  Validator::validate_matched_args
[      clap::parse::validator]  Validator::validate_matched_args:iter:[hash: 41CB8A11A30AC4B8]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "bar",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator]  Validator::validate_arg_num_vals
[      clap::parse::validator]  Validator::validate_arg_values: arg="foo"
[      clap::parse::validator]  Validator::validate_arg_num_occurs: "foo"=1
[    clap::parse::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[[hash: 59636393CFFBFE5F]]
bar
```

---

_Label `C-bug` added by @nbvdkamp on 2022-03-31 21:45_

---

_Comment by @epage on 2022-04-01 01:33_

As long as the assert is happening in debug mode regardless of what flags get passed in, this is expected behavior.

Asserts are generally a technique for communicating a development error as opposed to a runtime error so long as they are exclusively a development error and don't get in the way of delivering mission critical software. 

Debug-only asserts are something done to ensure the development-only assets do not impact end-user performance so long as testing is able to ensure they are never hit or that the release code can handle them not asserting.

Clap has been using debug asserts for a while but to help developers get quicker feedback on these, we added `Command::debug_assert` for running in tests for clap v3 and put it in our migration guide and in the [tutorial](https://github.com/clap-rs/clap/blob/v3.1.7/examples/tutorial_builder/README.md#tips).

As this is expected behavior, I'm going to go ahead and close.  If my assumption earlier is wrong or if there is something I missed, feel free to let us know!

---

_Closed by @epage on 2022-04-01 01:33_

---

_Comment by @nbvdkamp on 2022-04-01 12:16_

Oh I see now, I was developing my app in release builds because testing it is too slow in debug and assumed that this was a bug because it only showed up when I went to debug something and worked as I expected before. Thanks for the quick response.

---
