```yaml
number: 1405
title: "`requires_all` on SubCommand names causes a fatal error"
type: issue
state: closed
author: drozdziak1
labels: []
assignees: []
created_at: 2019-01-25T22:09:34Z
updated_at: 2020-02-01T19:17:09Z
url: https://github.com/clap-rs/clap/issues/1405
synced_at: 2026-01-12T16:14:10Z
```

# `requires_all` on SubCommand names causes a fatal error

---

_@drozdziak1_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version
1.33 nightly
* Use the output of `rustc -V`

### Affected Version of clap
2.32.0


### Bug or Feature Request Summary
The title sums it up pretty well.

### Expected Behavior Summary
**tl;dr: This behavior may be intended.**
I have args common for certain subcommands. I'd expect mentioning those subcommands in `requires_all` would deny the use of those options  unless with the listed subcommands.

I understand why this might be a misuse of the library, it's just the error is fatal and I thought this might not be the way you'd like `clap` to handle my weird ideas.

### Actual Behavior Summary
I get a fatal error encouraging me to write this issue :P

### Sample Code or Link to Sample Code
My `App`:
```rust
    let cli_matches = App::new("pinreq-matrix")
        .version(env!("CARGO_PKG_VERSION"))
        .about("Request a pin for an IPFS hash")
        .arg(
            Arg::with_name("homeserver")
                .short("s")
                .long("server")
                .value_name("HOMESERVER")
                .help("The Matrix homeserver to use; has to be HTTPS")
                .default_value("https://matrix.org")
                .requires_all(&["request", "listen"]),
        )
        .arg(
            Arg::with_name("room_alias")
                .help("The Matrix room to use; has to exist and be public")
                .short("r")
                .long("room")
                .value_name("ROOM_ALIAS")
                .default_value(DEFAULT_PINREQ_MATRIX_ROOM_ALIAS)
                .requires_all(&["request", "listen"]),
        )
        .subcommand(
            SubCommand::with_name("request")
                .about("Request pinning of the specified hash")
                .arg(
                    Arg::with_name("ipfs_hash")
                        .help("The IPFS/IPNS hash to pin-request")
                        .required(true)
                        .index(1),
                ),
        )
        .subcommand(SubCommand::with_name("listen").about("Listen for pin requests"))
        .get_matches();
```

### Debug output
**Note:** The line 41 mentioned in the backtrace is where `get_matches` is called for my `App` (pasted above).

<details>
<summary> Debug Output </summary>
<pre>
<code>

DEBUG:clap:Parser::add_subcommand: term_w=None, name=request
DEBUG:clap:Parser::add_subcommand: term_w=None, name=listen
DEBUG:clap:Parser::propagate_settings: self=pinreq-matrix, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: sc=request, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
), g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: self=request, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: sc=listen, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
), g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: self=listen, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Parser::add_defaults:iter:homeserver: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:homeserver: has default vals
DEBUG:clap:Parser::add_defaults:iter:homeserver: wasn't used
DEBUG:clap:Parser::add_val_to_arg; arg=homeserver, val="https://matrix.org"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."https://matrix.org"
DEBUG:clap:Parser::groups_for_arg: name=homeserver
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=homeserver
DEBUG:clap:Parser::add_defaults:iter:room_alias: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:room_alias: has default vals
DEBUG:clap:Parser::add_defaults:iter:room_alias: wasn't used
DEBUG:clap:Parser::add_val_to_arg; arg=room_alias, val="ipfs-pinreq"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."ipfs-pinreq"
DEBUG:clap:Parser::groups_for_arg: name=room_alias
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=room_alias
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_blacklist:iter:homeserver;
DEBUG:clap:Parser::groups_for_arg: name=homeserver
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Validator::validate_blacklist:iter:room_alias;
DEBUG:clap:Parser::groups_for_arg: name=room_alias
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:homeserver: vals=[
    "https://matrix.org"
]
DEBUG:clap:Validator::validate_arg_num_vals:homeserver
DEBUG:clap:Validator::validate_arg_values: arg="homeserver"
DEBUG:clap:Validator::validate_arg_requires:homeserver;
DEBUG:clap:Validator::missing_required_error: extra=Some("request")
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Validator::missing_required_error: reqs=[
    "request"
]
DEBUG:clap:usage::get_required_usage_from: reqs=["request"], extra=Some("request")
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=["request"]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["request"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::get_required_usage_from:iter:request:
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/kbknapp/clap-rs/issues', src/libcore/option.rs:1038:5
note: Run with `RUST_BACKTRACE=1` environment variable to display a backtrace.
 ✘ ⚙ drozdziak1@Arcydiakon  ~/rust/pinreq   master ●✚  RUST_BACKTRACE=1 cargo run --bin pinreq-matrix
    Finished dev [unoptimized + debuginfo] target(s) in 0.22s
     Running `target/debug/pinreq-matrix`
DEBUG:clap:Parser::add_subcommand: term_w=None, name=request
DEBUG:clap:Parser::add_subcommand: term_w=None, name=listen
DEBUG:clap:Parser::propagate_settings: self=pinreq-matrix, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: sc=request, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
), g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: self=request, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: sc=listen, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
), g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: self=listen, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Parser::add_defaults:iter:homeserver: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:homeserver: has default vals
DEBUG:clap:Parser::add_defaults:iter:homeserver: wasn't used
DEBUG:clap:Parser::add_val_to_arg; arg=homeserver, val="https://matrix.org"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."https://matrix.org"
DEBUG:clap:Parser::groups_for_arg: name=homeserver
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=homeserver
DEBUG:clap:Parser::add_defaults:iter:room_alias: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:room_alias: has default vals
DEBUG:clap:Parser::add_defaults:iter:room_alias: wasn't used
DEBUG:clap:Parser::add_val_to_arg; arg=room_alias, val="ipfs-pinreq"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."ipfs-pinreq"
DEBUG:clap:Parser::groups_for_arg: name=room_alias
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=room_alias
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_blacklist:iter:homeserver;
DEBUG:clap:Parser::groups_for_arg: name=homeserver
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Validator::validate_blacklist:iter:room_alias;
DEBUG:clap:Parser::groups_for_arg: name=room_alias
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:homeserver: vals=[
    "https://matrix.org"
]
DEBUG:clap:Validator::validate_arg_num_vals:homeserver
DEBUG:clap:Validator::validate_arg_values: arg="homeserver"
DEBUG:clap:Validator::validate_arg_requires:homeserver;
DEBUG:clap:Validator::missing_required_error: extra=Some("request")
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Validator::missing_required_error: reqs=[
    "request"
]
DEBUG:clap:usage::get_required_usage_from: reqs=["request"], extra=Some("request")
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=["request"]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["request"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::get_required_usage_from:iter:request:
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/kbknapp/clap-rs/issues', src/libcore/option.rs:1038:5
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
             at src/libstd/sys/unix/backtrace/tracing/gcc_s.rs:39
   1: std::sys_common::backtrace::_print
             at src/libstd/sys_common/backtrace.rs:70
   2: std::panicking::default_hook::{{closure}}
             at src/libstd/sys_common/backtrace.rs:58
             at src/libstd/panicking.rs:200
   3: std::panicking::default_hook
             at src/libstd/panicking.rs:215
   4: std::panicking::rust_panic_with_hook
             at src/libstd/panicking.rs:478
   5: std::panicking::continue_panic_fmt
             at src/libstd/panicking.rs:385
   6: rust_begin_unwind
             at src/libstd/panicking.rs:312
   7: core::panicking::panic_fmt
             at src/libcore/panicking.rs:85
   8: core::option::expect_failed
             at src/libcore/option.rs:1038
   9: <core::option::Option<T>>::expect
             at /rustc/4c2be9c97fb60a01c545b8e8fa61e4247ae5c9b2/src/libcore/option.rs:312
  10: clap::app::usage::get_required_usage_from::{{closure}}
             at /home/drozdziak1/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/usage.rs:457
  11: <core::option::Option<T>>::unwrap_or_else
             at /rustc/4c2be9c97fb60a01c545b8e8fa61e4247ae5c9b2/src/libcore/option.rs:386
  12: clap::app::usage::get_required_usage_from
             at /home/drozdziak1/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/usage.rs:454
  13: clap::app::validator::Validator::missing_required_error
             at /home/drozdziak1/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/validator.rs:551
  14: clap::app::validator::Validator::validate_arg_requires
             at /home/drozdziak1/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/validator.rs:436
  15: clap::app::validator::Validator::validate_matched_args
             at /home/drozdziak1/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/validator.rs:285
  16: clap::app::validator::Validator::validate
             at /home/drozdziak1/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/validator.rs:77
  17: clap::app::parser::Parser::get_matches_with
             at /home/drozdziak1/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/parser.rs:1199
  18: clap::app::App::get_matches_from_safe_borrow
             at /home/drozdziak1/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/mod.rs:1633
  19: clap::app::App::get_matches_from
             at /home/drozdziak1/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/mod.rs:1513
  20: clap::app::App::get_matches
             at /home/drozdziak1/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/mod.rs:1455
  21: pinreq_matrix::main
             at src/pinreq_matrix.rs:41
  22: std::rt::lang_start::{{closure}}
             at /rustc/4c2be9c97fb60a01c545b8e8fa61e4247ae5c9b2/src/libstd/rt.rs:64
  23: std::panicking::try::do_call
             at src/libstd/rt.rs:49
             at src/libstd/panicking.rs:297
  24: __rust_maybe_catch_panic
             at src/libpanic_unwind/lib.rs:92
  25: std::rt::lang_start_internal
             at src/libstd/panicking.rs:276
             at src/libstd/panic.rs:388
             at src/libstd/rt.rs:48
  26: std::rt::lang_start
             at /rustc/4c2be9c97fb60a01c545b8e8fa61e4247ae5c9b2/src/libstd/rt.rs:64
  27: main
  28: __libc_start_main
  29: _start


</code>
</pre>
</details>


---

_Comment by @drozdziak1 on 2019-01-26 12:14_

There's no way someone might want to use clap that way, I misunderstood what `requires_all()` does, closing.

---

_Closed by @CreepySkeleton on 2020-02-01 15:14_

---

_Label `T: RFC / question` added by @pksunkara on 2020-02-01 19:17_

---
