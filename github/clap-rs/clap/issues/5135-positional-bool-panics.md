---
number: 5135
title: Positional bool panics
type: issue
state: closed
author: gierens
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2023-09-25T10:37:13Z
updated_at: 2023-09-25T15:19:17Z
url: https://github.com/clap-rs/clap/issues/5135
synced_at: 2026-01-07T13:12:20-06:00
---

# Positional bool panics

---

_Issue opened by @gierens on 2023-09-25 10:37_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

stable-x86_64-unknown-linux-gnu unchanged - rustc 1.72.0 (5680fa18f 2023-08-23)

### Clap Version

clap v4.4.4 and clap_derive 4.4.2

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser)]
#[clap(name = "MyApp", version = "1.0")]
struct Args {
    #[clap(help = "Enable or disable something")]
    enabled: bool,
}

fn main() {
    let _ = Args::parse();
}
```


### Steps to reproduce the bug with the above code

`cargo run` panics while `cargo build` does not complain about anything.

### Actual Behaviour

```console
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/clap-bug-positional-bool`
thread 'main' panicked at 'Argument 'enabled` is positional, it must take a value', /home/sandro/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.4/src/builder/debug_asserts.rs:747:9
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

### Expected Behaviour

Ideally this should work as if `enabled` were some other type like `i64` for example, or this should already fail on compilation rather than during runtime.

### Additional Context

I'm working on a CLI client that has the command `user tfa <id> <enabled>` to either enable or disable TFA for a user. Due to this panic this is not possible however, or at least not with a positional bool.

Or is there any more argument for the attribute needed to accomplish this?

### Debug Output

With debug and backtrace:
```console
$ RUST_BACKTRACE=1 cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/clap-bug-positional-bool`
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="MyApp"
[clap_builder::builder::command]Command::_propagate:MyApp
[clap_builder::builder::command]Command::_check_help_and_version:MyApp expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_propagate_global_args:MyApp
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:enabled
thread 'main' panicked at 'Argument 'enabled` is positional, it must take a value', /home/sandro/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.4/src/builder/debug_asserts.rs:747:9
stack backtrace:
   0: rust_begin_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:593:5
   1: core::panicking::panic_fmt
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panicking.rs:67:14
   2: clap_builder::builder::debug_asserts::assert_arg
             at /home/sandro/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.4/src/builder/debug_asserts.rs:747:9
   3: clap_builder::builder::debug_asserts::assert_app
             at /home/sandro/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.4/src/builder/debug_asserts.rs:58:9
   4: clap_builder::builder::command::Command::_build_self
             at /home/sandro/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.4/src/builder/command.rs:3984:13
   5: clap_builder::builder::command::Command::_do_parse
             at /home/sandro/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.4/src/builder/command.rs:3855:9
   6: clap_builder::builder::command::Command::try_get_matches_from_mut
             at /home/sandro/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.4/src/builder/command.rs:791:9
   7: clap_builder::builder::command::Command::get_matches_from
             at /home/sandro/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.4/src/builder/command.rs:662:9
   8: clap_builder::builder::command::Command::get_matches
             at /home/sandro/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.4/src/builder/command.rs:571:9
   9: clap_builder::derive::Parser::parse
             at /home/sandro/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.4/src/derive.rs:27:27
  10: clap_bug_positional_bool::main
             at ./src/main.rs:12:13
  11: core::ops::function::FnOnce::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/ops/function.rs:250:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

---

_Label `C-bug` added by @gierens on 2023-09-25 10:37_

---

_Label `A-derive` added by @epage on 2023-09-25 11:54_

---

_Comment by @epage on 2023-09-25 11:59_

The relevant piece of documentation is the [arg types](https://docs.rs/clap/latest/clap/_derive/index.html#arg-types).  What you need to do is override the implicit `action = ArgAction::SetTrue` with `action = ArgAction::Set`.

`SetTrue` with flags is common enough that I wouldn't want to change that.  I'm also concerned with the communication problems of specializing this based on positional or not.  We could move this error to being a compilation error but then we start duplicating a lot of work (see also #3864).

Closing in favor of #4467 which is tracking this.

---

_Closed by @epage on 2023-09-25 11:59_

---

_Comment by @gierens on 2023-09-25 15:01_

Ah alright, thanks for the quick and precise response. Yeah, `ArgAction::SetTrue` is definitely more common than positional bools and thus makes sense as default.

While I am satisfied with all the infos now, I still fear that more people could fall into this trap and especially with a rare thing such as positional bools easily assume this runtime warning is an actual bug and report it. Would it maybe make sense to add a hint regarding the arg types to the assert message here? Or does it handle to many other cases as well for this to be too specific?

---

_Referenced in [clap-rs/clap#5136](../../clap-rs/clap/pulls/5136.md) on 2023-09-25 15:19_

---

_Comment by @epage on 2023-09-25 15:19_

Good idea.  #5136 is at least a slight improvement

---
