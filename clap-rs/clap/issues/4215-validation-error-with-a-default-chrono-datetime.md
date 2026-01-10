---
number: 4215
title: "Validation error with a default `chrono::DateTime` and custom parser function"
type: issue
state: closed
author: affanshahid
labels:
  - C-bug
assignees: []
created_at: 2022-09-14T20:49:03Z
updated_at: 2022-09-14T21:18:24Z
url: https://github.com/clap-rs/clap/issues/4215
synced_at: 2026-01-10T01:27:51Z
---

# Validation error with a default `chrono::DateTime` and custom parser function

---

_Issue opened by @affanshahid on 2022-09-14 20:49_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.63.0 (4b91a6ea7 2022-08-08)

### Clap Version

3.2.21

### Minimal reproducible code

```rust
use chrono::DateTime;
use chrono::TimeZone;
use chrono::Utc;
use clap::Parser;

#[derive(Parser, Debug)]
#[clap(author, version, about, long_about=None)]
struct Args {
    #[clap(
        short, 
        long, 
        value_parser=DateTime::parse_from_rfc3339, 
        default_value_t=Utc::now(),
    )]
    start: DateTime<Utc>,
}

fn main() {
    let args = Args::parse();
}
```


### Steps to reproduce the bug with the above code

`cargo run -- --help`

### Actual Behaviour

I get the following error

```
thread 'main' panicked at 'Argument `start`'s default_value="2022-09-14 20:30:14.397521 UTC" failed validation: error: Invalid value "2022-09-14 20:30:14.397521 UTC" for '--start <START>': input contains invalid characters
```

I'm not sure why this is happening but my guess is that it is calling `fmt` using the `Display` trait on the default DateTime I have provided (which prints the date in a different format) and then trying to parse it using the function I have provided which expects the RFC 3339 format.

### Expected Behaviour

There should be no errors and it should use the default date as is.

### Additional Context

_No response_

### Debug Output

```
[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="finny"
[      clap::builder::command] 	Command::_propagate:finny
[      clap::builder::command] 	Command::_check_help_and_version: finny
[      clap::builder::command] 	Command::_propagate_global_args:finny
[      clap::builder::command] 	Command::_derive_display_order:finny
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:help
[clap::builder::debug_asserts] 	Arg::_debug_asserts:version
[clap::builder::debug_asserts] 	Arg::_debug_asserts:contacts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:start
[      clap::builder::command] 	Command::color: Color setting...
[      clap::builder::command] 	Auto
thread 'main' panicked at 'Argument `start`'s default_value="2022-09-14 20:46:12.909474 UTC" failed validation: error: Invalid value "2022-09-14 20:46:12.909474 UTC" for '--start <START>': input contains invalid characters

For more information try --help

Backtrace:
   0: backtrace::backtrace::libunwind::trace
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.66/src/backtrace/libunwind.rs:93:5
      backtrace::backtrace::trace_unsynchronized
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.66/src/backtrace/mod.rs:66:5
   1: backtrace::backtrace::trace
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.66/src/backtrace/mod.rs:53:14
   2: backtrace::capture::Backtrace::create
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.66/src/capture.rs:176:9
   3: backtrace::capture::Backtrace::new
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.66/src/capture.rs:140:22
   4: clap::error::Backtrace::new
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/error/mod.rs:1120:19
   5: clap::error::Error::new
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/error/mod.rs:181:28
   6: clap::error::Error::value_validation
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/error/mod.rs:495:9
   7: <F as clap::builder::value_parser::TypedValueParser>::parse_ref::{{closure}}
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/value_parser.rs:709:13
   8: core::result::Result<T,E>::map_err
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/core/src/result.rs:855:27
   9: <F as clap::builder::value_parser::TypedValueParser>::parse_ref
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/value_parser.rs:705:21
  10: <P as clap::builder::value_parser::AnyValueParser>::parse_ref
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/value_parser.rs:575:21
  11: clap::builder::value_parser::ValueParser::parse_ref
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/value_parser.rs:233:9
  12: clap::builder::debug_asserts::assert_defaults
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/debug_asserts.rs:844:34
  13: clap::builder::debug_asserts::assert_arg
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/debug_asserts.rs:705:5
  14: clap::builder::debug_asserts::assert_app
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/debug_asserts.rs:70:9
  15: clap::builder::command::App::_build_self
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/command.rs:4340:13
  16: clap::builder::command::App::_do_parse
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/command.rs:4198:9
  17: clap::builder::command::App::try_get_matches_from_mut
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/command.rs:730:9
  18: clap::builder::command::App::get_matches_from
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/command.rs:600:9
  19: clap::builder::command::App::get_matches
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/command.rs:512:9
  20: clap::derive::Parser::parse
             at /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/derive.rs:81:27
  21: finny::main
             at src/main.rs:30:16
  22: core::ops::function::FnOnce::call_once
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/core/src/ops/function.rs:248:5
  23: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/std/src/sys_common/backtrace.rs:122:18
  24: std::rt::lang_start::{{closure}}
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/std/src/rt.rs:145:18
  25: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/core/src/ops/function.rs:280:13
      std::panicking::try::do_call
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/std/src/panicking.rs:492:40
      std::panicking::try
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/std/src/panicking.rs:456:19
      std::panic::catch_unwind
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/std/src/panic.rs:137:14
      std::rt::lang_start_internal::{{closure}}
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/std/src/rt.rs:128:48
      std::panicking::try::do_call
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/std/src/panicking.rs:492:40
      std::panicking::try
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/std/src/panicking.rs:456:19
      std::panic::catch_unwind
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/std/src/panic.rs:137:14
      std::rt::lang_start_internal
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/std/src/rt.rs:128:20
  26: std::rt::lang_start
             at /rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f/library/std/src/rt.rs:144:17
  27: _main

', /Users/affan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.21/src/builder/debug_asserts.rs:845:13
```

---

_Label `C-bug` added by @affanshahid on 2022-09-14 20:49_

---

_Comment by @epage on 2022-09-14 21:18_

> I'm not sure why this is happening but my guess is that it is calling fmt using the Display trait on the default DateTime I have provided (which prints the date in a different format) and then trying to parse it using the function I have provided which expects the RFC 3339 format.

Yes, `default_value_t` relies on the `Display` implementation. Whether you used `default_value_t` / `Display` or used `default_value = ""`, we provide asserts to help catch mistakes earlier development so you don't have to wait until you've exhaustively tested in your CLI.

In this case, you will need to use `default_value = "..."` which will require a `&'static str`.  You can get one with either leaking memory or `once_cell`.

Since it sounds like things are working as expected, I'm going to go ahead and close for now.  If there was something I'm missing, let me know and we re-open this!

---

_Closed by @epage on 2022-09-14 21:18_

---
