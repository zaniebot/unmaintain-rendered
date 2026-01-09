---
number: 3608
title: "Argument with `last` option set, still requires specifying its index"
type: issue
state: closed
author: kamilogorek
labels:
  - C-bug
assignees: []
created_at: 2022-04-04T12:57:51Z
updated_at: 2022-04-04T14:52:52Z
url: https://github.com/clap-rs/clap/issues/3608
synced_at: 2026-01-07T13:12:19-06:00
---

# Argument with `last` option set, still requires specifying its index

---

_Issue opened by @kamilogorek on 2022-04-04 12:57_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.61.0-nightly

### Clap Version

3.1.6

### Minimal reproducible code

```rust
use clap::{Arg, Command};

fn main() {
    let cmd = Command::new("app")
        .arg(Arg::new("script").value_name("SCRIPT").index(1))
        .arg(
            Arg::new("args")
                .value_name("ARGS")
                .takes_value(true)
                .multiple_values(true)
                .last(true),
        );
    let matches = cmd.get_matches();
    println!("{:?}", matches);
}
```


### Steps to reproduce the bug with the above code

```terminal
cargo run app parser.sh -- strict raw
```

### Actual Behaviour

It should correctly accept multiple additional arguments after `--` delimiter.

### Expected Behaviour

```terminal
thread 'main' panicked at 'Command app: Argument 'script' has the same index as 'args' and they are both positional arguments

         Use Arg::multiple_values(true) to allow one positional argument to take multiple values', /Users/kamilogorek/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.8/src/build/debug_asserts.rs:116:17
```

### Additional Context

This is currently fixable by adding `index(2)` to the argument that has `.last(true)` set. However, the docs state that it should be already done by adding the `last` option itself:

> This arg is the last, or final, positional argument (i.e. has the highest index) 

### Debug Output

```terminal
[        clap::build::command]  App::_do_parse
[        clap::build::command]  App::_build
[        clap::build::command]  App::_propagate:app
[        clap::build::command]  App::_check_help_and_version: app
[        clap::build::command]  App::_check_help_and_version: Removing generated version
[        clap::build::command]  App::_propagate_global_args:app
[        clap::build::command]  App::_derive_display_order:app
[  clap::build::debug_asserts]  Command::_debug_asserts
[  clap::build::debug_asserts]  Arg::_debug_asserts:help
[  clap::build::debug_asserts]  Arg::_debug_asserts:script
thread 'main' panicked at 'Command app: Argument 'script' has the same index as 'args' and they are both positional arguments

         Use Arg::multiple_values(true) to allow one positional argument to take multiple values', /Users/kamilogorek/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.8/src/build/debug_asserts.rs:116:17
stack backtrace:
   0: rust_begin_unwind
             at /rustc/03918badd33d255de806b4a9a8aa75b031ed0738/library/std/src/panicking.rs:584:5
   1: core::panicking::panic_fmt
             at /rustc/03918badd33d255de806b4a9a8aa75b031ed0738/library/core/src/panicking.rs:143:14
   2: clap::build::debug_asserts::assert_app
             at /Users/kamilogorek/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.8/src/build/debug_asserts.rs:116:17
   3: clap::build::command::App::_build
             at /Users/kamilogorek/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.8/src/build/command.rs:4101:13
   4: clap::build::command::App::_do_parse
             at /Users/kamilogorek/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.8/src/build/command.rs:3965:9
   5: clap::build::command::App::try_get_matches_from_mut
             at /Users/kamilogorek/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.8/src/build/command.rs:681:9
   6: clap::build::command::App::get_matches_from
             at /Users/kamilogorek/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.8/src/build/command.rs:551:9
   7: clap::build::command::App::get_matches
             at /Users/kamilogorek/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.8/src/build/command.rs:463:9
   8: clap_bug::main
             at ./src/main.rs:13:19
   9: core::ops::function::FnOnce::call_once
             at /rustc/03918badd33d255de806b4a9a8aa75b031ed0738/library/core/src/ops/function.rs:227:5
```

---

_Label `C-bug` added by @kamilogorek on 2022-04-04 12:57_

---

_Comment by @epage on 2022-04-04 13:33_

I'm assuming if you do not set `index(1)` it will work.

We will infer indexes.  Our counter starts at 1 and is only incremented on positional arguments without an index.  For us to do otherwise would require us to collect a list of all set indexes and skip over those, hoping that that is what the user intended.

Overall, I think we should remove `index` from all of our examples as it should be rare that someone needs to set it manually.

---

_Comment by @kamilogorek on 2022-04-04 13:44_

I can confirm that it works without explicit indexes. I'm fine with that solution, so feel free to close this issue if you think is the intended behavior. Thanks!

---

_Referenced in [clap-rs/clap#3609](../../clap-rs/clap/pulls/3609.md) on 2022-04-04 13:54_

---

_Closed by @epage on 2022-04-04 14:52_

---
