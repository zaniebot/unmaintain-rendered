---
number: 6164
title: "Arg: `action(ArgAction::Count)` conflicts with `help_header` and other oddities causing panics and alarming memory usage"
type: issue
state: closed
author: RachaelAva1024
labels:
  - C-bug
  - S-triage
assignees: []
created_at: 2025-10-29T08:37:27Z
updated_at: 2025-10-29T18:15:24Z
url: https://github.com/clap-rs/clap/issues/6164
synced_at: 2026-01-07T13:12:20-06:00
---

# Arg: `action(ArgAction::Count)` conflicts with `help_header` and other oddities causing panics and alarming memory usage

---

_Issue opened by @RachaelAva1024 on 2025-10-29 08:37_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.86.0 (05f9846f8 2025-03-31) (built from a source tarball)    (NixOS 25.05)

### Clap Version

4.5.50

### Minimal reproducible code

```rust
use clap::{crate_name, Arg, ArgAction, ArgMatches, Command};

fn main() {
    let clap_matches: ArgMatches = Command::new(crate_name!())
        .arg(
            Arg::new("test")
                .short('t')
                .help("This combination of args breaks Clap")
                .help_heading("Misc") // This causes breakage.
                .action(ArgAction::Count) // This causes breakage.
        )
        .arg_required_else_help(true) // This causes problems when enabled.
        .get_matches();

    dbg!(clap_matches);
}
```

Please refer to the sample Cargo project attached for more verbose details.

### Steps to reproduce the bug with the above code

```shell
cargo r
```

### Actual Behaviour

```shell
RUST_BACKTRACE=full cargo r
```

---

```
thread 'main' panicked at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/output/help_template.rs:588:24:
attempt to subtract with overflow
stack backtrace:
   0:     0x55ebd3889a70 - <std::sys::backtrace::BacktraceLock::print::DisplayBacktrace as core::fmt::Display>::fmt::h9edbd6e38a8b0805
   1:     0x55ebd389cfe3 - core::fmt::write::h7b1248e5e0c79c78
   2:     0x55ebd3861593 - std::io::Write::write_fmt::h5e301665499081bf
   3:     0x55ebd3889913 - std::sys::backtrace::BacktraceLock::print::h4a386d2ef944f43e
   4:     0x55ebd388610c - std::panicking::default_hook::{{closure}}::h61b7aa0fc15f236b
   5:     0x55ebd3886019 - std::panicking::default_hook::h2d21379b0b23a14f
   6:     0x55ebd388657f - std::panicking::rust_panic_with_hook::h100726ba9570b85a
   7:     0x55ebd3889e06 - std::panicking::begin_panic_handler::{{closure}}::h141712493bfacf0c
   8:     0x55ebd3889c79 - std::sys::backtrace::__rust_end_short_backtrace::h891003731531c924
   9:     0x55ebd38861ad - rust_begin_unwind
  10:     0x55ebd3761510 - core::panicking::panic_fmt::h1df68d570cb2382b
  11:     0x55ebd3761b17 - core::panicking::panic_const::panic_const_sub_overflow::hea66bde0f212d609
  12:     0x55ebd37ca745 - clap_builder::output::help_template::HelpTemplate::align_to_about::h50406f3697de5aad
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/output/help_template.rs:588:24
  13:     0x55ebd37c9cf9 - clap_builder::output::help_template::HelpTemplate::write_arg::h513cc3b4213043b0
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/output/help_template.rs:523:9
  14:     0x55ebd37c9953 - clap_builder::output::help_template::HelpTemplate::write_args::h47fbd9c42663a912
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/output/help_template.rs:510:13
  15:     0x55ebd37c93a7 - clap_builder::output::help_template::HelpTemplate::write_all_args::h70d4567da8355e7b
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/output/help_template.rs:457:21
  16:     0x55ebd37c72ea - clap_builder::output::help_template::HelpTemplate::write_templated_help::hf2c8b1dfe8452ace
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/output/help_template.rs:215:25
  17:     0x55ebd37c6a8c - clap_builder::output::help_template::AutoHelp::write_help::h35f14ca236ec75e9
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/output/help_template.rs:61:9
  18:     0x55ebd379d913 - clap_builder::output::help::write_help::hf12986314e68fc59
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/output/help.rs:23:17
  19:     0x55ebd379b7b1 - clap_builder::builder::command::Command::write_help_err::h0d06fd32a479c820
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/builder/command.rs:5135:9
  20:     0x55ebd380da31 - clap_builder::parser::validator::Validator::validate::h67db63bfe4855700
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/parser/validator.rs:35:31
  21:     0x55ebd37eeaf1 - clap_builder::parser::parser::Parser::get_matches_with::hd5326c068729b6d6
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/parser/parser.rs:71:9
  22:     0x55ebd379420b - clap_builder::builder::command::Command::_do_parse::h452841b0c72bcf2f
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/builder/command.rs:4359:29
  23:     0x55ebd37928eb - clap_builder::builder::command::Command::try_get_matches_from_mut::h09a34d2943a1479a
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/builder/command.rs:909:9
  24:     0x55ebd3763844 - clap_builder::builder::command::Command::get_matches_from::ha48a95e7c2b889d3
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/builder/command.rs:764:9
  25:     0x55ebd3762c98 - clap_builder::builder::command::Command::get_matches::h0654b8ac4522ba5d
                               at /home/rachaelava/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.50/src/builder/command.rs:665:9
  26:     0x55ebd376278d - clap_bug_replica::main::h16154a0746866c80
                               at /tmp/clap-bug-replica/src/main.rs:7:36
  27:     0x55ebd3762ecb - core::ops::function::FnOnce::call_once::h4b66a4bdb0df4222
                               at /build/rustc-1.86.0-src/library/core/src/ops/function.rs:250:5
  28:     0x55ebd3762f6e - std::sys::backtrace::__rust_begin_short_backtrace::h963842d67ad0d1a7
                               at /build/rustc-1.86.0-src/library/std/src/sys/backtrace.rs:152:18
  29:     0x55ebd3763711 - std::rt::lang_start::{{closure}}::h3b49352068d2d8ad
                               at /build/rustc-1.86.0-src/library/std/src/rt.rs:199:18
  30:     0x55ebd3868935 - std::rt::lang_start_internal::he3cad277a2bdfe30
  31:     0x55ebd37636f7 - std::rt::lang_start::h0d5b19de7c2db00f
                               at /build/rustc-1.86.0-src/library/std/src/rt.rs:198:5
  32:     0x55ebd3762a8e - main
  33:     0x7fc38ae2a47e - __libc_start_call_main
  34:     0x7fc38ae2a539 - __libc_start_main_alias_1
  35:     0x55ebd3761cd5 - _start
  36:                0x0 - <unknown>
```

### Expected Behaviour

I expect to run `cargo run` and get my help menu.

It also shouldn't ***continuously eat my memory*** when running `cargo run -r`

### Additional Context

Below is a tarball containing a sample Cargo project with the issue, as well as panic dumps, and more verbose context.
[clap-bug-replica.tar.gz](https://github.com/user-attachments/files/23205433/clap-bug-replica.tar.gz)

Although I built the sample Cargo project inside /tmp which is a tmpfs mount, I originally discovered the "attempt to subtract with overflow" panic inside a non-volatile BTRFS volume.

### Debug Output

Clap 4.5.50 does not support the debug feature.

---

_Label `C-bug` added by @RachaelAva1024 on 2025-10-29 08:37_

---

_Label `S-triage` added by @RachaelAva1024 on 2025-10-29 08:37_

---

_Comment by @epage on 2025-10-29 13:52_

As a cargo script:
```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { version = "4", features = ["derive", "cargo"] }
---

use clap::{crate_name, Arg, ArgAction, ArgMatches, Command};

fn main() {
    let clap_matches: ArgMatches = Command::new(crate_name!())
        .arg(
            Arg::new("test")
                .short('t')
                .help("This combination of args breaks Clap")
                .help_heading("Misc") // This causes breakage.
                .action(ArgAction::Count), // This causes breakage.
        )
        .arg_required_else_help(true) // This causes problems when enabled.
        .get_matches();

    dbg!(clap_matches);
}

```

---

_Comment by @epage on 2025-10-29 13:58_

The bug is that when there is a help heading with short-only arguments, our padding calculations for the long argument / value get thrown off, causing this.

---

_Comment by @epage on 2025-10-29 14:12_

The count gets off because we aren't accounting for `...`

---

_Referenced in [clap-rs/clap#6165](../../clap-rs/clap/pulls/6165.md) on 2025-10-29 14:13_

---

_Closed by @epage on 2025-10-29 14:19_

---

_Comment by @RachaelAva1024 on 2025-10-29 18:01_

Ahhh, I see! That would explain the underflow! Thanks for taking care of this! Will test the new patch and report back.

---

_Comment by @RachaelAva1024 on 2025-10-29 18:14_

The new 4.5.51 update fixes this issue on my end, and it all works exactly as intended now! I no longer get the "attempt to subtract with overflow" panic. The "Argument 'test' is positional and it must take a value but action is Count" panic also no longer occurs when I remove the `.short` method. I think we can call this a success! Thanks for your help, and I'm so happy this was resolved so quickly!

Cheers!

---
