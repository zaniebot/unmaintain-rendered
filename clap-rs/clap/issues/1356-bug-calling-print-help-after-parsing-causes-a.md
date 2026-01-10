---
number: 1356
title: "Bug: Calling print_help after parsing causes a panic if global() is used."
type: issue
state: closed
author: mikeswoods
labels: []
assignees: []
created_at: 2018-10-09T16:03:39Z
updated_at: 2020-02-01T16:03:33Z
url: https://github.com/clap-rs/clap/issues/1356
synced_at: 2026-01-10T01:26:49Z
---

# Bug: Calling print_help after parsing causes a panic if global() is used.

---

_Issue opened by @mikeswoods on 2018-10-09 16:03_

If `App::print_help()` ius called after parsing with `Arg::get_matches_from_safe_borrow` a panic occurs. This only happens if one of the arguments has  `global(true)` set.

### Rust Version

`rustc 1.29.0 (aa3ca1994 2018-09-11)`

### Affected Version of clap

2.32.0

### Bug or Feature Request Summary

Bug: If `App::print_help()` ius called after parsing with `Arg::get_matches_from_safe_borrow` a panic occurs. This only happens if one of the arguments has  `global(true)` set.

### Expected Behavior Summary

No panic

### Actual Behavior Summary

The program panics with

```
% RUST_BACKTRACE=1 cargo run -- foo                                                                                                                                                                                                         (git)-[master]
    Finished dev [unoptimized + debuginfo] target(s) in 0.22s
     Running `target/debug/clap-test foo`
ArgMatches { args: {}, subcommand: Some(SubCommand { name: "foo", matches: ArgMatches { args: {}, subcommand: None, usage: Some("USAGE:\n    clap-test foo [OPTIONS]") } }), usage: Some("USAGE:\n    clap-test [OPTIONS] [SUBCOMMAND]") }
thread 'main' panicked at 'Non-unique argument name: output is already in use', /Users/mike/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/parser.rs:174:9
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
             at libstd/sys/unix/backtrace/tracing/gcc_s.rs:49
   1: std::sys_common::backtrace::print
             at libstd/sys_common/backtrace.rs:71
             at libstd/sys_common/backtrace.rs:59
   2: std::panicking::default_hook::{{closure}}
             at libstd/panicking.rs:211
   3: std::panicking::default_hook
             at libstd/panicking.rs:227
   4: <std::panicking::begin_panic::PanicPayload<A> as core::panic::BoxMeUp>::get
             at libstd/panicking.rs:475
   5: std::collections::hash::table::TaggedHashUintPtr::set_tag
             at /Users/travis/build/rust-lang/rust/src/libstd/panicking.rs:409
   6: clap::app::parser::Parser::debug_asserts
             at /Users/mike/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/parser.rs:174
   7: clap::app::parser::Parser::implied_settings
             at /Users/mike/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/parser.rs:319
   8: clap::app::parser::Parser::verify_positionals::{{closure}}
             at /Users/mike/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/parser.rs:651
   9: <std::ffi::os_str::OsStr as core::cmp::PartialEq<std::ffi::os_str::OsString>>::eq
             at /Users/mike/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/mod.rs:1161
  10: clap_test::main
             at src/main.rs:45
  11: std::rt::lang_start::{{closure}}
             at /Users/travis/build/rust-lang/rust/src/libstd/rt.rs:74
  12: std::panicking::try::do_call
             at libstd/rt.rs:59
             at libstd/panicking.rs:310
  13: panic_unwind::dwarf::eh::read_encoded_pointer
             at libpanic_unwind/lib.rs:105
  14: std::sys_common::cleanup
             at libstd/panicking.rs:289
             at libstd/panic.rs:392
             at libstd/rt.rs:58
  15: std::rt::lang_start
             at /Users/travis/build/rust-lang/rust/src/libstd/rt.rs:74
  16: clap_test::main
```

### Steps to Reproduce the issue

The following code will reproduce the issue:

```rust
extern crate clap;

fn main() {
    let mut app = clap::App::new(env!("CARGO_PKG_NAME"))
                .version(env!("CARGO_PKG_VERSION"))
                .author(env!("CARGO_PKG_AUTHORS"))
                .about("Just a test")
	       .arg(clap::Arg::with_name("output")
                         .value_name("output")
                         .short("o")
                         .long("output")
                         .takes_value(true)
                         .global(true) // false works 
                         .possible_value("simple")
                         .possible_value("rich")
                         .help("Sets the output format"))
        .subcommand(clap::SubCommand::with_name("foo")
                    .about("foo"))
        .subcommand(clap::SubCommand::with_name("bar")
                    .about("bar")
                    .arg(clap::Arg::with_name("oof")
                         .short("z")
                         .long("oof")
                         .help("oof")))
        .subcommand(clap::SubCommand::with_name("baz")
                    .about("baz")
                    .arg(clap::Arg::with_name("zap")
                         .value_name("zap")
                         .takes_value(true)
                         .required(true)
                         .index(1)
                         .help("zap")));

    let parse = app.get_matches_from_safe_borrow(::std::env::args_os());
    let args = match parse {
    	Ok(args) => args,
    	Err(e) => {
    		println!("{}", e.message);
    		::std::process::exit(1);
    	}
    };

    println!("{:?}", args);

    app.print_help();
}
```

Afterward, run `cargo run -- foo` to reproduce.


---

_Comment by @remram44 on 2018-11-10 01:59_

Or even just calling `print_help()` twice.

Seems related to #1076 and #1348: basically, this `global()` thing wasn't really tested with the rest of the code, and seems to break with every non-trivial setup.

---

_Comment by @CreepySkeleton on 2020-02-01 16:03_

Closing as duplicate of #1601

---

_Closed by @CreepySkeleton on 2020-02-01 16:03_

---

_Label `T: duplicate` added by @CreepySkeleton on 2020-02-01 16:03_

---
