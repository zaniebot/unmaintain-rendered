---
number: 4086
title: default_value_if derive macro has no effect
type: issue
state: closed
author: grebnetiew
labels:
  - C-bug
assignees: []
created_at: 2022-08-17T11:08:33Z
updated_at: 2022-08-25T13:47:02Z
url: https://github.com/clap-rs/clap/issues/4086
synced_at: 2026-01-10T01:27:50Z
---

# default_value_if derive macro has no effect

---

_Issue opened by @grebnetiew on 2022-08-17 11:08_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.62.0 (a8314ef7d 2022-06-27)

### Clap Version

3.2.17

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser)]
struct Config {
    #[clap(long)]
    aaa: String,
    #[clap(long, default_value_if("aaa", None, Some("b")))]
    bbb: String,
}

fn main() {
    // Be sure to enable the 'derive' feature of clap in Cargo.toml
    let _ = Config::parse();
}
```


### Steps to reproduce the bug with the above code

@thomasTNO
```
cargo run -- --aaa Hello
```

### Actual Behaviour

```
error: The following required arguments were not provided:
    --bbb <BBB>

USAGE:
    bug --aaa <AAA> --bbb <BBB>

For more information try --help
```

### Expected Behaviour

The program ends successfully.

### Additional Context

The equivalent argument definition in the builder pattern does work:
```rust
use clap::{Arg, Command};

fn main() {
    let _ = Command::new("app")
        .arg(Arg::new("aaa").long("aaa").takes_value(true))
        .arg(
            Arg::new("bbb")
                .long("bbb")
                .takes_value(true)
                .default_value_if("aaa", None, Some("b")),
        )
        .get_matches();
}
```

My colleague @thomasTNO first discovered this bug and can also provide clarifications.

### Debug Output

<details>
  <summary>Hidden because of the length - click here to expand</summary>
<pre>
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/bug --aaa Hello`
[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="bug"
[      clap::builder::command] 	Command::_propagate:bug
[      clap::builder::command] 	Command::_check_help_and_version: bug
[      clap::builder::command] 	Command::_check_help_and_version: Removing generated version
[      clap::builder::command] 	Command::_propagate_global_args:bug
[      clap::builder::command] 	Command::_derive_display_order:bug
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Arg::_debug_asserts:aaa
[clap::builder::debug_asserts] 	Arg::_debug_asserts:bbb
[clap::builder::debug_asserts] 	Command::_verify_positionals
[        clap::parser::parser] 	Parser::get_matches_with
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--aaa")' ([45, 45, 97, 97, 97])
[        clap::parser::parser] 	Parser::possible_subcommand: arg=Ok("--aaa")
[        clap::parser::parser] 	Parser::get_matches_with: sc=None
[        clap::parser::parser] 	Parser::parse_long_arg
[        clap::parser::parser] 	Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser] 	Parser::parse_long_arg: Found valid arg or flag '--aaa <AAA>'
[        clap::parser::parser] 	Parser::parse_long_arg("aaa"): Found an arg with value 'None'
[        clap::parser::parser] 	Parser::parse_opt_value; arg=aaa, val=None, has_eq=false
[        clap::parser::parser] 	Parser::parse_opt_value; arg.settings=ArgFlags(REQUIRED | TAKES_VAL)
[        clap::parser::parser] 	Parser::parse_opt_value; Checking for val...
[        clap::parser::parser] 	Parser::parse_opt_value: More arg vals required...
[        clap::parser::parser] 	Parser::get_matches_with: After parse_long_arg Opt(aaa)
[        clap::parser::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("Hello")' ([72, 101, 108, 108, 111])
[        clap::parser::parser] 	Parser::split_arg_values; arg=aaa, val=RawOsStr("Hello")
[        clap::parser::parser] 	Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=aaa, resolved=0, pending=1
[        clap::parser::parser] 	Parser::resolve_pending: id=aaa
[        clap::parser::parser] 	Parser::react action=StoreValue, identifier=Some(Long), source=CommandLine
[        clap::parser::parser] 	Parser::react: cur_idx:=1
[        clap::parser::parser] 	Parser::remove_overrides: id=aaa
[   clap::parser::arg_matcher] 	ArgMatcher::start_occurrence_of_arg: id=aaa
[      clap::builder::command] 	Command::groups_for_arg: id=aaa
[        clap::parser::parser] 	Parser::push_arg_values: ["Hello"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=2
[      clap::builder::command] 	Command::groups_for_arg: id=aaa
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=aaa, resolved=1, pending=0
[        clap::parser::parser] 	Parser::add_defaults
[        clap::parser::parser] 	Parser::add_defaults:iter:help:
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:help: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:aaa:
[        clap::parser::parser] 	Parser::add_default_value:iter:aaa: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:aaa: doesn't have default vals
[        clap::parser::parser] 	Parser::add_defaults:iter:bbb:
[        clap::parser::parser] 	Parser::add_default_value:iter:bbb: doesn't have default missing vals
[        clap::parser::parser] 	Parser::add_default_value: has conditional defaults
[        clap::parser::parser] 	Parser::split_arg_values; arg=bbb, val=RawOsStr("b")
[        clap::parser::parser] 	Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[        clap::parser::parser] 	Parser::react action=StoreValue, identifier=None, source=DefaultValue
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id=bbb, source=DefaultValue
[      clap::builder::command] 	Command::groups_for_arg: id=bbb
[        clap::parser::parser] 	Parser::push_arg_values: ["b"]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=3
[      clap::builder::command] 	Command::groups_for_arg: id=bbb
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=bbb, resolved=1, pending=0
[     clap::parser::validator] 	Validator::validate
[     clap::parser::validator] 	Validator::validate_conflicts
[     clap::parser::validator] 	Validator::validate_exclusive
[     clap::parser::validator] 	Validator::validate_conflicts::iter: id=aaa
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg=aaa
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::validate_required: required=ChildGraph([Child { id: aaa, children: [] }, Child { id: bbb, children: [] }])
[     clap::parser::validator] 	Validator::gather_requires
[     clap::parser::validator] 	Validator::gather_requires:iter:aaa
[     clap::parser::validator] 	Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator] 	Validator::validate_required:iter:aog=bbb
[     clap::parser::validator] 	Validator::validate_required:iter: This is an arg
[     clap::parser::validator] 	Validator::is_missing_required_ok: bbb
[     clap::parser::validator] 	Conflicts::gather_conflicts: arg=bbb
[      clap::builder::command] 	Command::groups_for_arg: id=bbb
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id=bbb, conflicts=[]
[      clap::builder::command] 	Command::groups_for_arg: id=aaa
[     clap::parser::validator] 	Conflicts::gather_direct_conflicts id=aaa, conflicts=[]
[     clap::parser::validator] 	Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator] 	Validator::missing_required_error; incl=[]
[     clap::parser::validator] 	Validator::missing_required_error: reqs=ChildGraph([Child { id: aaa, children: [] }, Child { id: bbb, children: [] }])
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=true, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={aaa, bbb}
[         clap::output::usage] 	Usage::get_required_usage_from:iter:aaa arg is_present=true
[         clap::output::usage] 	Usage::get_required_usage_from:iter:bbb arg is_present=false
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val={"--bbb <BBB>"}
[     clap::parser::validator] 	Validator::missing_required_error: req_args=[
    "--bbb <BBB>",
]
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_smart_usage
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[aaa], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={aaa, bbb}
[         clap::output::usage] 	Usage::get_required_usage_from:iter:aaa arg is_present=false
[         clap::output::usage] 	Usage::get_required_usage_from:iter:bbb arg is_present=false
[         clap::output::usage] 	Usage::get_required_usage_from:iter:aaa arg is_present=false
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val={"--aaa <AAA>", "--bbb <BBB>"}
[      clap::builder::command] 	Command::color: Color setting...
[      clap::builder::command] 	Auto
error: The following required arguments were not provided:
    --bbb <BBB>

USAGE:
    bug --aaa <AAA> --bbb <BBB>

For more information try --help
</pre>
</details>

---

_Label `C-bug` added by @grebnetiew on 2022-08-17 11:08_

---

_Comment by @grebnetiew on 2022-08-17 11:17_

Running `cargo expand` on the minimal reproducible code does produce a builder version with `default_value_if`:

```rust
                        .value_parser(clap::builder::ValueParser::string())
                        .action(clap::ArgAction::StoreValue);
                    let arg = arg.long("bbb").default_value_if("aaa", None, Some("b"));
                    arg
                });
```

Nevertheless, the default value is not used if `--aaa` is present.

<details><summary>Click here for the full main.rs with expanded derive macros</summary><pre>
#![feature(prelude_import)]
#[prelude_import]
use std::prelude::rust_2021::*;
#[macro_use]
extern crate std;
use clap::Parser;
struct Config {
    #[clap(long)]
    aaa: String,
    #[clap(long, default_value_if("aaa", None, Some("b")))]
    bbb: String,
}
impl clap::Parser for Config {}
#[allow(dead_code, unreachable_code, unused_variables, unused_braces)]
#[allow(
    clippy::style,
    clippy::complexity,
    clippy::pedantic,
    clippy::restriction,
    clippy::perf,
    clippy::deprecated,
    clippy::nursery,
    clippy::cargo,
    clippy::suspicious_else_formatting,
)]
#[deny(clippy::correctness)]
#[allow(deprecated)]
impl clap::CommandFactory for Config {
    fn into_app<'b>() -> clap::Command<'b> {
        let __clap_app = clap::Command::new("bug");
        <Self as clap::Args>::augment_args(__clap_app)
    }
    fn into_app_for_update<'b>() -> clap::Command<'b> {
        let __clap_app = clap::Command::new("bug");
        <Self as clap::Args>::augment_args_for_update(__clap_app)
    }
}
#[allow(dead_code, unreachable_code, unused_variables, unused_braces)]
#[allow(
    clippy::style,
    clippy::complexity,
    clippy::pedantic,
    clippy::restriction,
    clippy::perf,
    clippy::deprecated,
    clippy::nursery,
    clippy::cargo,
    clippy::suspicious_else_formatting,
)]
#[deny(clippy::correctness)]
impl clap::FromArgMatches for Config {
    fn from_arg_matches(
        __clap_arg_matches: &clap::ArgMatches,
    ) -> ::std::result::Result<Self, clap::Error> {
        Self::from_arg_matches_mut(&mut __clap_arg_matches.clone())
    }
    fn from_arg_matches_mut(
        __clap_arg_matches: &mut clap::ArgMatches,
    ) -> ::std::result::Result<Self, clap::Error> {
        #![allow(deprecated)]
        let v = Config {
            aaa: __clap_arg_matches
                .get_one::<String>("aaa")
                .map(|s| ::std::ops::Deref::deref(s))
                .ok_or_else(|| clap::Error::raw(
                    clap::ErrorKind::MissingRequiredArgument,
                    {
                        let res = ::alloc::fmt::format(
                            ::core::fmt::Arguments::new_v1(
                                &["The following required argument was not provided: "],
                                &[::core::fmt::ArgumentV1::new_display(&"aaa")],
                            ),
                        );
                        res
                    },
                ))
                .and_then(|s| {
                    ::std::str::FromStr::from_str(s)
                        .map_err(|err| clap::Error::raw(
                            clap::ErrorKind::ValueValidation,
                            {
                                let res = ::alloc::fmt::format(
                                    ::core::fmt::Arguments::new_v1(
                                        &["Invalid value for ", ": "],
                                        &[
                                            ::core::fmt::ArgumentV1::new_display(&"aaa"),
                                            ::core::fmt::ArgumentV1::new_display(&err),
                                        ],
                                    ),
                                );
                                res
                            },
                        ))
                })?,
            bbb: __clap_arg_matches
                .get_one::<String>("bbb")
                .map(|s| ::std::ops::Deref::deref(s))
                .ok_or_else(|| clap::Error::raw(
                    clap::ErrorKind::MissingRequiredArgument,
                    {
                        let res = ::alloc::fmt::format(
                            ::core::fmt::Arguments::new_v1(
                                &["The following required argument was not provided: "],
                                &[::core::fmt::ArgumentV1::new_display(&"bbb")],
                            ),
                        );
                        res
                    },
                ))
                .and_then(|s| {
                    ::std::str::FromStr::from_str(s)
                        .map_err(|err| clap::Error::raw(
                            clap::ErrorKind::ValueValidation,
                            {
                                let res = ::alloc::fmt::format(
                                    ::core::fmt::Arguments::new_v1(
                                        &["Invalid value for ", ": "],
                                        &[
                                            ::core::fmt::ArgumentV1::new_display(&"bbb"),
                                            ::core::fmt::ArgumentV1::new_display(&err),
                                        ],
                                    ),
                                );
                                res
                            },
                        ))
                })?,
        };
        ::std::result::Result::Ok(v)
    }
    fn update_from_arg_matches(
        &mut self,
        __clap_arg_matches: &clap::ArgMatches,
    ) -> ::std::result::Result<(), clap::Error> {
        self.update_from_arg_matches_mut(&mut __clap_arg_matches.clone())
    }
    fn update_from_arg_matches_mut(
        &mut self,
        __clap_arg_matches: &mut clap::ArgMatches,
    ) -> ::std::result::Result<(), clap::Error> {
        #![allow(deprecated)]
        if __clap_arg_matches.contains_id("aaa") {
            #[allow(non_snake_case)]
            let aaa = &mut self.aaa;
            *aaa = __clap_arg_matches
                .get_one::<String>("aaa")
                .map(|s| ::std::ops::Deref::deref(s))
                .ok_or_else(|| clap::Error::raw(
                    clap::ErrorKind::MissingRequiredArgument,
                    {
                        let res = ::alloc::fmt::format(
                            ::core::fmt::Arguments::new_v1(
                                &["The following required argument was not provided: "],
                                &[::core::fmt::ArgumentV1::new_display(&"aaa")],
                            ),
                        );
                        res
                    },
                ))
                .and_then(|s| {
                    ::std::str::FromStr::from_str(s)
                        .map_err(|err| clap::Error::raw(
                            clap::ErrorKind::ValueValidation,
                            {
                                let res = ::alloc::fmt::format(
                                    ::core::fmt::Arguments::new_v1(
                                        &["Invalid value for ", ": "],
                                        &[
                                            ::core::fmt::ArgumentV1::new_display(&"aaa"),
                                            ::core::fmt::ArgumentV1::new_display(&err),
                                        ],
                                    ),
                                );
                                res
                            },
                        ))
                })?;
        }
        if __clap_arg_matches.contains_id("bbb") {
            #[allow(non_snake_case)]
            let bbb = &mut self.bbb;
            *bbb = __clap_arg_matches
                .get_one::<String>("bbb")
                .map(|s| ::std::ops::Deref::deref(s))
                .ok_or_else(|| clap::Error::raw(
                    clap::ErrorKind::MissingRequiredArgument,
                    {
                        let res = ::alloc::fmt::format(
                            ::core::fmt::Arguments::new_v1(
                                &["The following required argument was not provided: "],
                                &[::core::fmt::ArgumentV1::new_display(&"bbb")],
                            ),
                        );
                        res
                    },
                ))
                .and_then(|s| {
                    ::std::str::FromStr::from_str(s)
                        .map_err(|err| clap::Error::raw(
                            clap::ErrorKind::ValueValidation,
                            {
                                let res = ::alloc::fmt::format(
                                    ::core::fmt::Arguments::new_v1(
                                        &["Invalid value for ", ": "],
                                        &[
                                            ::core::fmt::ArgumentV1::new_display(&"bbb"),
                                            ::core::fmt::ArgumentV1::new_display(&err),
                                        ],
                                    ),
                                );
                                res
                            },
                        ))
                })?;
        }
        ::std::result::Result::Ok(())
    }
}
#[allow(dead_code, unreachable_code, unused_variables, unused_braces)]
#[allow(
    clippy::style,
    clippy::complexity,
    clippy::pedantic,
    clippy::restriction,
    clippy::perf,
    clippy::deprecated,
    clippy::nursery,
    clippy::cargo,
    clippy::suspicious_else_formatting,
)]
#[deny(clippy::correctness)]
impl clap::Args for Config {
    fn augment_args<'b>(__clap_app: clap::Command<'b>) -> clap::Command<'b> {
        {
            let __clap_app = __clap_app;
            let __clap_app = __clap_app
                .arg({
                    #[allow(deprecated)]
                    let arg = clap::Arg::new("aaa")
                        .takes_value(true)
                        .value_name("AAA")
                        .required(true && clap::ArgAction::StoreValue.takes_values())
                        .validator(|s| {
                            ::std::str::FromStr::from_str(s).map(|_: String| ())
                        })
                        .value_parser(clap::builder::ValueParser::string())
                        .action(clap::ArgAction::StoreValue);
                    let arg = arg.long("aaa");
                    arg
                });
            let __clap_app = __clap_app
                .arg({
                    #[allow(deprecated)]
                    let arg = clap::Arg::new("bbb")
                        .takes_value(true)
                        .value_name("BBB")
                        .required(true && clap::ArgAction::StoreValue.takes_values())
                        .validator(|s| {
                            ::std::str::FromStr::from_str(s).map(|_: String| ())
                        })
                        .value_parser(clap::builder::ValueParser::string())
                        .action(clap::ArgAction::StoreValue);
                    let arg = arg.long("bbb").default_value_if("aaa", None, Some("b"));
                    arg
                });
            __clap_app
        }
    }
    fn augment_args_for_update<'b>(__clap_app: clap::Command<'b>) -> clap::Command<'b> {
        {
            let __clap_app = __clap_app;
            let __clap_app = __clap_app
                .arg({
                    #[allow(deprecated)]
                    let arg = clap::Arg::new("aaa")
                        .takes_value(true)
                        .value_name("AAA")
                        .required(false && clap::ArgAction::StoreValue.takes_values())
                        .validator(|s| {
                            ::std::str::FromStr::from_str(s).map(|_: String| ())
                        })
                        .value_parser(clap::builder::ValueParser::string())
                        .action(clap::ArgAction::StoreValue);
                    let arg = arg.long("aaa");
                    arg
                });
            let __clap_app = __clap_app
                .arg({
                    #[allow(deprecated)]
                    let arg = clap::Arg::new("bbb")
                        .takes_value(true)
                        .value_name("BBB")
                        .required(false && clap::ArgAction::StoreValue.takes_values())
                        .validator(|s| {
                            ::std::str::FromStr::from_str(s).map(|_: String| ())
                        })
                        .value_parser(clap::builder::ValueParser::string())
                        .action(clap::ArgAction::StoreValue);
                    let arg = arg.long("bbb").default_value_if("aaa", None, Some("b"));
                    arg
                });
            __clap_app
        }
    }
}
fn main() {
    let _ = Config::parse();
}
</pre></details>

---

_Comment by @epage on 2022-08-17 16:26_

The builder is equivalent of
```rust
#!/usr/bin/env -S rust-script --debug

//! ```cargo
//! [dependencies]
//! clap = { version = "3.2", features = ["derive", "debug"] }
//! ```

use clap::{Arg, Command};

fn main() {
    let _ = Command::new("app")
        .arg(Arg::new("aaa").long("aaa").takes_value(true).required(true))
        .arg(
            Arg::new("bbb")
                .long("bbb")
                .takes_value(true)
                .default_value_if("aaa", None, Some("b"))
                .required(true),
        )
        .get_matches();
}
```
Note the `required(true)`

So to fix this, you need to override the `required(true)` call:
```rust
#!/usr/bin/env -S rust-script --debug

//! ```cargo
//! [dependencies]
//! clap = { version = "3.2", features = ["derive", "debug"] }
//! ```

use clap::Parser;

#[derive(Parser)]
struct Config {
    #[clap(long)]
    aaa: String,
    #[clap(long, default_value_if("aaa", None, Some("b")), required = false)]
    bbb: String,
}

fn main() {
    // Be sure to enable the 'derive' feature of clap in Cargo.toml
    let _ = Config::parse();
}
```

---

_Comment by @Jimmy-Z on 2022-08-25 11:00_

I also encountered this today:
```rust
use clap::Parser;

#[derive(Parser, Debug)]
struct Args {
    #[clap(short)]
    a: bool,
    #[clap(short, default_value_t = 0, default_value_if("a", Some("true"), Some("42")))]
    b: u8,
}

fn main(){
    println!("{:?}", Args::parse());
    println!("{:?}", Args::parse_from(["", "-a"]));
}
```
output
```
Args { a: false, b: 0 }
Args { a: true, b: 0 }
```

I'd expect the second b to be 42.

on playground:
https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=7364fa83ea195e952288e5c52c1dfb5b


---

_Comment by @Jimmy-Z on 2022-08-25 11:10_

@epage 's `required = false` fix above does fix the original case but not in the case I presented above, should I open a new issue?

---

_Comment by @epage on 2022-08-25 11:52_

The default action for a `bool` / flag is to only mark it as present / not-present.  To make that work, you would do
```rust
#!/usr/bin/env -S rust-script --debug

//! ```cargo
//! [dependencies]
//! clap = { version = "3.2", features = ["derive", "debug"] }
//! ```

use clap::Parser;

#[derive(Parser, Debug)]
struct Args {
    #[clap(short)]
    a: bool,
    #[clap(short, default_value_t = 0, default_value_if("a", None, Some("42")))]
    b: u8,
}

fn main() {
    let a = Args::parse();
    dbg!(a);
}
```

In clap v4 we are changing that behavior and you can opt-in by adding a defaulted `action` attribute
```rust
#!/usr/bin/env -S rust-script --debug

//! ```cargo
//! [dependencies]
//! clap = { version = "3.2", features = ["derive", "debug"] }
//! ```

use clap::Parser;

#[derive(Parser, Debug)]
struct Args {
    #[clap(short, action)]
    a: bool,
    #[clap(
        short,
        default_value_t = 0,
        default_value_if("a", Some("true"), Some("42"))
    )]
    b: u8,
}

fn main() {
    let a = Args::parse();
    dbg!(a);
}
```

---

_Comment by @Jimmy-Z on 2022-08-25 12:11_

Thanks for the workaround but isn't it a little counter-intuitive? after all the 2nd a is true.

---

_Comment by @epage on 2022-08-25 13:46_

This is a side effect of the Builder API that underlies the Derive API.  I agree with "present"/"not-present" not being ideal which is one of the reasons we are moving away from it.

---

_Closed by @epage on 2022-08-25 13:47_

---
