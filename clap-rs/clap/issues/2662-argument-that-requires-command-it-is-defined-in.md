---
number: 2662
title: Argument that requires command it is defined in is pointless (causes internal error)
type: issue
state: closed
author: LizardWizzard
labels:
  - C-bug
assignees: []
created_at: 2021-08-03T11:16:10Z
updated_at: 2021-08-03T11:17:07Z
url: https://github.com/clap-rs/clap/issues/2662
synced_at: 2026-01-10T01:27:22Z
---

# Argument that requires command it is defined in is pointless (causes internal error)

---

_Issue opened by @LizardWizzard on 2021-08-03 11:16_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.53.0 (53cb7b09b 2021-06-17)

### Clap Version

2.33.3

### Minimal reproducible code

```rust
use clap::{App, AppSettings, Arg, SubCommand};

fn main() {
    let matches = App::new("My CLI")
        .setting(AppSettings::ArgRequiredElseHelp)
        .subcommand(
            SubCommand::with_name("init")
                .about("Init")
                .arg(
                    Arg::with_name("enable-foo")
                        .long("enable-foo")
                        .takes_value(false)
                        .help("Enable foo")
                        .requires("init"), // is obviously incorrect
                ),
        ).get_matches();
    dbg!(matches);
}
```


### Steps to reproduce the bug with the above code

cargo run --example clap_bug -- init --enable-foo

### Actual Behaviour

```
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/clap-rs/clap/issues', /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/usage.rs:475:22
stack backtrace:
   0: rust_begin_unwind
             at /rustc/53cb7b09b00cbea8754ffb78e7e3cb521cb8af4b/library/std/src/panicking.rs:493:5
   1: core::panicking::panic_fmt
             at /rustc/53cb7b09b00cbea8754ffb78e7e3cb521cb8af4b/library/core/src/panicking.rs:92:14
   2: core::option::expect_failed
             at /rustc/53cb7b09b00cbea8754ffb78e7e3cb521cb8af4b/library/core/src/option.rs:1241:5
   3: core::option::Option<T>::expect
             at /Users/dmitry/.rustup/toolchains/stable-x86_64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:349:21
   4: clap::app::usage::get_required_usage_from::{{closure}}
             at /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/usage.rs:473:17
   5: core::option::Option<T>::unwrap_or_else
             at /Users/dmitry/.rustup/toolchains/stable-x86_64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:427:21
   6: clap::app::usage::get_required_usage_from
             at /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/usage.rs:470:19
   7: clap::app::validator::Validator::missing_required_error
             at /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/validator.rs:561:13
   8: clap::app::validator::Validator::validate_arg_requires
             at /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/validator.rs:445:28
   9: clap::app::validator::Validator::validate_matched_args
             at /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/validator.rs:296:17
  10: clap::app::validator::Validator::validate
             at /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/validator.rs:80:9
  11: clap::app::parser::Parser::get_matches_with
             at /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/parser.rs:1249:9
  12: clap::app::parser::Parser::parse_subcommand
             at /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/parser.rs:1382:13
  13: clap::app::parser::Parser::get_matches_with
             at /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/parser.rs:1215:17
  14: clap::app::App::get_matches_from_safe_borrow
             at /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/mod.rs:1639:25
  15: clap::app::App::get_matches_from
             at /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/mod.rs:1519:9
  16: clap::app::App::get_matches
             at /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/mod.rs:1460:9
  17: clap_bug::main
             at ./examples/clap_bug.rs:4:19
  18: core::ops::function::FnOnce::call_once
             at /Users/dmitry/.rustup/toolchains/stable-x86_64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:227:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

### Expected Behaviour

I would expect an error message that points out that argument that requires command it is defined in is pointless

### Additional Context

_No response_

### Debug Output

```
DEBUG:clap:Parser::add_subcommand: term_w=None, name=init
DEBUG:clap:Parser::propagate_settings: self=My CLI, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=init, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=init, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:Parser::get_matches_with: Begin parsing '"init"' ([105, 110, 105, 116])
DEBUG:clap:Parser::is_new_arg:"init":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::possible_subcommand: arg="init"
DEBUG:clap:Parser::get_matches_with: possible_sc=true, sc=Some("init")
DEBUG:clap:Parser::parse_subcommand;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:Parser::parse_subcommand: About to parse sc=init
DEBUG:clap:Parser::parse_subcommand: sc settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--enable-foo"' ([45, 45, 101, 110, 97, 98, 108, 101, 45, 102, 111, 111])
DEBUG:clap:Parser::is_new_arg:"--enable-foo":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="--enable-foo"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:Parser::parse_long_arg: Found valid flag '--enable-foo'
DEBUG:clap:Parser::check_for_help_and_version_str;
DEBUG:clap:Parser::check_for_help_and_version_str: Checking if --enable-foo is help or version...Neither
DEBUG:clap:Parser::parse_flag;
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=enable-foo
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=enable-foo
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser:get_matches_with: After parse_long_arg Flag
DEBUG:clap:ArgMatcher::process_arg_overrides:Some("enable-foo");
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_blacklist:iter:enable-foo;
DEBUG:clap:Parser::groups_for_arg: name=enable-foo
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:enable-foo: vals=[]
DEBUG:clap:Validator::validate_arg_requires:enable-foo;
DEBUG:clap:Validator::missing_required_error: extra=Some("init")
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Validator::missing_required_error: reqs=[
    "init",
]
DEBUG:clap:usage::get_required_usage_from: reqs=["init"], extra=Some("init")
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=["init"]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["init"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::get_required_usage_from:iter:init:
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/clap-rs/clap/issues', /Users/dmitry/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/usage.rs:475:22

```

---

_Label `T: bug` added by @LizardWizzard on 2021-08-03 11:16_

---

_Comment by @pksunkara on 2021-08-03 11:17_

Fixed in master

---

_Closed by @pksunkara on 2021-08-03 11:17_

---
