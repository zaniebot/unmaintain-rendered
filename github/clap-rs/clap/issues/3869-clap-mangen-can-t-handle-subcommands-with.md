---
number: 3869
title: "clap_mangen can't handle subcommands with arguments that conflict with global opts"
type: issue
state: closed
author: sunshowers
labels:
  - C-bug
assignees: []
created_at: 2022-06-25T20:37:38Z
updated_at: 2022-07-11T20:12:05Z
url: https://github.com/clap-rs/clap/issues/3869
synced_at: 2026-01-07T13:12:20-06:00
---

# clap_mangen can't handle subcommands with arguments that conflict with global opts

---

_Issue opened by @sunshowers on 2022-06-25 20:37_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.61.0 (fe5b13d68 2022-05-18)

### Clap Version

3.2.6

### Minimal reproducible code

```rust
//# clap = { version = "3.2.6", features = ["derive"] }
//# clap_mangen = "0.1.6"

use clap::{CommandFactory, Parser, Subcommand};
use clap_mangen::Man;

#[derive(Debug, Parser)]
struct MyApp {
    #[clap(long)]
    myflag: bool,
    #[clap(subcommand)]
    subcommand: MyCommand,
}

#[derive(Debug, Subcommand)]
enum MyCommand {
    MySubcommand {
        #[clap(long, conflicts_with = "myflag")]
        subcommand_flag: bool,
    },
}

fn main() {
    let command = MyApp::command();

    let man = Man::new(command.clone());
    let mut buf = Vec::new();
    man.render(&mut buf).expect("rendering main page worked");

    for subcommand in command.get_subcommands() {
        let man = Man::new(subcommand.clone());
        let mut buf = Vec::new();
        man.render(&mut buf).expect("rendering subcommand worked");
    }
}
```


### Steps to reproduce the bug with the above code

`cargo run` (I use `cargo-play`)

### Actual Behaviour

```
thread 'main' panicked at 'Command my-subcommand: Argument or group 'myflag' specified in 'conflicts_with*' for 'subcommand-flag' does not exist', /opt/cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.6/src/builder/debug_asserts.rs:237:13
stack backtrace:
   0: rust_begin_unwind
             at /rustc/fe5b13d681f25ee6474be29d748c65adcd91f69e/library/std/src/panicking.rs:584:5
   1: core::panicking::panic_fmt
             at /rustc/fe5b13d681f25ee6474be29d748c65adcd91f69e/library/core/src/panicking.rs:143:14
   2: clap::builder::debug_asserts::assert_app
             at /opt/cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.6/src/builder/debug_asserts.rs:237:13
   3: clap::builder::command::App::_build_self
             at /opt/cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.6/src/builder/command.rs:4256:13
   4: clap::builder::command::App::_build_recursive
             at /opt/cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.6/src/builder/command.rs:4173:9
   5: clap::builder::command::App::_build_recursive
             at /opt/cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.6/src/builder/command.rs:4175:13
   6: clap::builder::command::App::build
             at /opt/cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.6/src/builder/command.rs:4168:9
   7: clap_mangen::Man::new
             at /opt/cargo/registry/src/github.com-1ecc6299db9ec823/clap_mangen-0.1.9/src/lib.rs:29:9
   8: pdyve54v1gmc8ygblwcr3ndpfy4e::main
             at /tmp/cargo-play.DYVe54V1GMC8ygbLwcr3nDPfY4e/src/main.rs:26:15
   9: core::ops::function::FnOnce::call_once
             at /rustc/fe5b13d681f25ee6474be29d748c65adcd91f69e/library/core/src/ops/function.rs:227:5
```

### Expected Behaviour

The man pages render correctly.

### Additional Context

Seems like maybe there should be a way to disable the `build` method call that happens at the top of `Man::new`.

### Debug Output

```
[      clap::builder::command]    Command::_build: name="pdyve54v1gmc8ygblwcr3ndpfy4e"
[      clap::builder::command]         Command::_propagate:pdyve54v1gmc8ygblwcr3ndpfy4e
[      clap::builder::command]         Command::_check_help_and_version: pdyve54v1gmc8ygblwcr3ndpfy4e
[      clap::builder::command]         Command::_check_help_and_version: Removing generated version
[      clap::builder::command]         Command::_check_help_and_version: Building help subcommand
[      clap::builder::command]         Command::_propagate_global_args:pdyve54v1gmc8ygblwcr3ndpfy4e
[      clap::builder::command]         Command::_propagate removing my-subcommand's help
[      clap::builder::command]         Command::_propagate pushing help to my-subcommand
[      clap::builder::command]         Command::_propagate removing help's help
[      clap::builder::command]         Command::_propagate pushing help to help
[      clap::builder::command]         Command::_derive_display_order:pdyve54v1gmc8ygblwcr3ndpfy4e
[      clap::builder::command]         Command::_derive_display_order:my-subcommand
[      clap::builder::command]         Command::_derive_display_order:help
[clap::builder::debug_asserts]         Command::_debug_asserts
[clap::builder::debug_asserts]         Arg::_debug_asserts:help
[clap::builder::debug_asserts]         Arg::_debug_asserts:myflag
[clap::builder::debug_asserts]         Command::_verify_positionals
[      clap::builder::command]         Command::_build: name="my-subcommand"
[      clap::builder::command]         Command::_propagate:my-subcommand
[      clap::builder::command]         Command::_check_help_and_version: my-subcommand
[      clap::builder::command]         Command::_check_help_and_version: Removing generated version
[      clap::builder::command]         Command::_propagate_global_args:my-subcommand
[      clap::builder::command]         Command::_derive_display_order:my-subcommand
[clap::builder::debug_asserts]         Command::_debug_asserts
[clap::builder::debug_asserts]         Arg::_debug_asserts:subcommand-flag
```

---

_Label `C-bug` added by @sunshowers on 2022-06-25 20:37_

---

_Referenced in [nextest-rs/nextest#312](../../nextest-rs/nextest/pulls/312.md) on 2022-06-25 20:40_

---

_Referenced in [clap-rs/clap#3365](../../clap-rs/clap/issues/3365.md) on 2022-06-25 20:46_

---

_Comment by @epage on 2022-07-11 20:12_

This is more general than just `clap_mangen`
```console
#!/usr/bin/env -S rust-script --debug

//! ```cargo
//! [dependencies]
//! clap = { version = "3.2.8", features = ["env", "derive"] }
//! ```

use clap::{CommandFactory, Parser, Subcommand};

#[derive(Debug, Parser)]
struct MyApp {
    #[clap(long)]
    myflag: bool,
    #[clap(subcommand)]
    subcommand: MyCommand,
}

#[derive(Debug, Subcommand)]
enum MyCommand {
    MySubcommand {
        #[clap(long, conflicts_with = "myflag")]
        subcommand_flag: bool,
    },
}

fn main() {
    let command = MyApp::command();
    command.debug_assert();
}
```
```console
thread 'main' panicked at 'Command my-subcommand: Argument or group 'myflag' specified in 'conflicts_with*' for 'subcommand-flag' does not exist', /home/epage/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.8/src/builder/debug_asserts.rs:237:13
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

or if you want to verify this happen in regular code
```rust
#!/usr/bin/env -S rust-script --debug

//! ```cargo
//! [dependencies]
//! clap = { version = "3.2.8", features = ["env", "derive"] }
//! ```

use clap::{Parser, Subcommand};

#[derive(Debug, Parser)]
struct MyApp {
    #[clap(long)]
    myflag: bool,
    #[clap(subcommand)]
    subcommand: MyCommand,
}

#[derive(Debug, Subcommand)]
enum MyCommand {
    MySubcommand {
        #[clap(long, conflicts_with = "myflag")]
        subcommand_flag: bool,
    },
}

fn main() {
    MyApp::parse();
}
```
```console
$ ./clap-3869.rs  my-subcommand
thread 'main' panicked at 'Command my-subcommand: Argument or group 'myflag' specified in 'conflicts_with*' for 'subcommand-flag' does not exist', /home/epage/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.2.8/src/builder/debug_asserts.rs:237:13
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Depending on what your intent is, you can either
- Mark `myflag` with `global = true` attribute
- Set `args_conflicts_with_subcommands = true` attribute on the command

If you actually need top-level arguments to conflict with a subxcommand's arguments, then that will unlikely be supported in clap directly (if nothing else, argument ids only exist for the current command level).  However, this can be checked manually or we are looking at more general validation plugins, see https://github.com/clap-rs/clap/issues/3008.

---

_Closed by @epage on 2022-07-11 20:12_

---
