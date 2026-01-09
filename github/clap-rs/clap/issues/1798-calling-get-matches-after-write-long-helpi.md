---
number: 1798
title: Calling get_matches() after write_long_helpI() reports Non-unique argument name
type: issue
state: closed
author: arucil
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2020-04-09T04:23:35Z
updated_at: 2020-04-27T08:32:18Z
url: https://github.com/clap-rs/clap/issues/1798
synced_at: 2026-01-07T13:12:19-06:00
---

# Calling get_matches() after write_long_helpI() reports Non-unique argument name

---

_Issue opened by @arucil on 2020-04-09 04:23_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

`rustc 1.44.0-nightly (6dee5f112 2020-04-06)`

### Code

```rust
use clap::*;
fn main() {
    let mut app = App::new("app")
        .arg(Arg::with_name("log")
            .global(true)
            .long("log"))
        .subcommand(SubCommand::with_name("sub"));

    app.write_long_help(&mut Vec::new()).unwrap();
    app.get_matches();
}
```

### Steps to reproduce the issue

1. Run `cargo run`
2. The program panics at the line where `get_matches()` is called:
<details>
<summary>Backtrace</summary>

```
thread 'main' panicked at 'Non-unique argument name: log is already in use', /home/xxx/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/clap-2.33.0/src/app/parser.rs:174:9
stack backtrace:
   0: backtrace::backtrace::libunwind::trace
             at /cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.46/src/backtrace/libunwind.rs:86
   1: backtrace::backtrace::trace_unsynchronized
             at /cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.46/src/backtrace/mod.rs:66
   2: std::sys_common::backtrace::_print_fmt
             at src/libstd/sys_common/backtrace.rs:78
   3: <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt
             at src/libstd/sys_common/backtrace.rs:59
   4: core::fmt::write
             at src/libcore/fmt/mod.rs:1069
   5: std::io::Write::write_fmt
             at src/libstd/io/mod.rs:1439
   6: std::sys_common::backtrace::_print
             at src/libstd/sys_common/backtrace.rs:62
   7: std::sys_common::backtrace::print
             at src/libstd/sys_common/backtrace.rs:49
   8: std::panicking::default_hook::{{closure}}
             at src/libstd/panicking.rs:198
   9: std::panicking::default_hook
             at src/libstd/panicking.rs:218
  10: std::panicking::rust_panic_with_hook
             at src/libstd/panicking.rs:511
  11: std::panicking::begin_panic
             at /home/xxx/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/src/libstd/panicking.rs:438
  12: clap::app::parser::Parser::debug_asserts
             at /home/xxx/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/clap-2.33.0/src/app/parser.rs:174
  13: clap::app::parser::Parser::add_arg_ref
             at /home/xxx/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/clap-2.33.0/src/app/parser.rs:319
  14: clap::app::parser::Parser::propagate_globals
             at /home/xxx/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/clap-2.33.0/src/app/parser.rs:651
  15: clap::app::App::get_matches_from_safe_borrow
             at /home/xxx/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/clap-2.33.0/src/app/mod.rs:1598
  16: clap::app::App::get_matches_from
             at /home/xxx/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/clap-2.33.0/src/app/mod.rs:1509
  17: clap::app::App::get_matches
             at /home/xxx/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/clap-2.33.0/src/app/mod.rs:1451
  18: test_clap::main
             at src/main.rs:10
  19: std::rt::lang_start::{{closure}}
             at /home/xxx/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/src/libstd/rt.rs:67
  20: std::rt::lang_start_internal::{{closure}}
             at src/libstd/rt.rs:52
  21: std::panicking::try::do_call
             at src/libstd/panicking.rs:331
  22: std::panicking::try
             at src/libstd/panicking.rs:274
  23: std::panic::catch_unwind
             at src/libstd/panic.rs:394
  24: std::rt::lang_start_internal
             at src/libstd/rt.rs:51
  25: std::rt::lang_start
             at /home/xxx/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/src/libstd/rt.rs:67
  26: main
  27: __libc_start_main
  28: _start
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```
</details>

### Affected Version of `clap*`

2.33.0

### Actual Behavior Summary

Calling `get_matches()` after calling the `write_long_help()` method of an `App` with a global argument and a subcommand panics, with the message 'Non-unique argument name: log is already in use'. But when I call `write_help()` instead of `write_long_help()`, the program runs fine.

### Expected Behavior Summary

I think `write_long_help()` should not affect the behavior of `get_matches()`.

---

_Label `T: bug` added by @arucil on 2020-04-09 04:23_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-09 06:15_

---

_Label `C: parsing` added by @pksunkara on 2020-04-12 21:51_

---

_Comment by @CreepySkeleton on 2020-04-27 08:32_

I can't reproduce it with master. Don't know what exactly fixed this, but something did. If somebody runs into this again, please create a new issue.

---

_Closed by @CreepySkeleton on 2020-04-27 08:32_

---
