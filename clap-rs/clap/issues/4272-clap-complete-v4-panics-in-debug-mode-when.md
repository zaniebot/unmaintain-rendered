```yaml
number: 4272
title: clap_complete v4 panics in debug mode when generating
type: issue
state: closed
author: alexheretic
labels:
  - C-bug
assignees: []
created_at: 2022-09-28T18:46:04Z
updated_at: 2022-09-28T21:25:09Z
url: https://github.com/clap-rs/clap/issues/4272
synced_at: 2026-01-12T16:14:15Z
```

# clap_complete v4 panics in debug mode when generating

---

_@alexheretic_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0 (a55dd71d5 2022-09-19)

### Clap Version

4.0.0

### Minimal reproducible code

completion-derive example: https://github.com/clap-rs/clap/blob/master/clap_complete/examples/completion-derive.rs

### Steps to reproduce the bug with the above code

```sh
cargo run --example completion-derive -- --generate=bash
```

### Actual Behaviour

```sh
$ RUST_BACKTRACE=1 cargo run --example completion-derive -- --generate=bash
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `/home/alex/.cargo/target/debug/examples/completion-derive --generate=bash`
thread 'main' panicked at 'Command value_hints_derive: Short option names must be unique for each argument, but '-h' is in use by both 'host' and 'help' (call `cmd.disable_help_flag(true)` to remove the auto-generated `--help`)', src/builder/debug_asserts.rs:121:17
stack backtrace:
   0: rust_begin_unwind
             at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/std/src/panicking.rs:584:5
   1: core::panicking::panic_fmt
             at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/core/src/panicking.rs:142:14
   2: clap::builder::debug_asserts::assert_app
             at /home/alex/project/clap/src/builder/debug_asserts.rs:121:17
   3: clap::builder::command::Command::_build_self
             at /home/alex/project/clap/src/builder/command.rs:3898:13
   4: clap::builder::command::Command::_do_parse
             at /home/alex/project/clap/src/builder/command.rs:3768:9
   5: clap::builder::command::Command::try_get_matches_from_mut
             at /home/alex/project/clap/src/builder/command.rs:708:9
   6: clap::builder::command::Command::get_matches_from
             at /home/alex/project/clap/src/builder/command.rs:578:9
   7: clap::builder::command::Command::get_matches
             at /home/alex/project/clap/src/builder/command.rs:490:9
   8: clap::derive::Parser::parse
             at /home/alex/project/clap/src/derive.rs:82:27
   9: completion_derive::main
             at ./examples/completion-derive.rs:62:15
  10: core::ops::function::FnOnce::call_once
             at /rustc/a55dd71d5fb0ec5a6a3a9e8c27b2127ba491ce52/library/core/src/ops/function.rs:248:5
```

### Expected Behaviour

It should not panic.

No panics in release mode and the output seems to be correct too.

### Additional Context

_No response_

### Debug Output

```
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="value_hints_derive"
[      clap::builder::command]  Command::_propagate:value_hints_derive
[      clap::builder::command]  Command::_check_help_and_version:value_hints_derive expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_propagate_global_args:value_hints_derive
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:generator
[clap::builder::debug_asserts]  Arg::_debug_asserts:unknown
[clap::builder::debug_asserts]  Arg::_debug_asserts:other
[clap::builder::debug_asserts]  Arg::_debug_asserts:path
[clap::builder::debug_asserts]  Arg::_debug_asserts:file
[clap::builder::debug_asserts]  Arg::_debug_asserts:dir
[clap::builder::debug_asserts]  Arg::_debug_asserts:exe
[clap::builder::debug_asserts]  Arg::_debug_asserts:cmd_name
[clap::builder::debug_asserts]  Arg::_debug_asserts:cmd
[clap::builder::debug_asserts]  Arg::_debug_asserts:command_with_args
[clap::builder::debug_asserts]  Arg::_debug_asserts:user
[clap::builder::debug_asserts]  Arg::_debug_asserts:host
thread 'main' panicked at 'Command value_hints_derive: Short option names must be unique for each argument, but '-h' is in use by both 'host' and 'help' (call `cmd.disable_help_flag(true)` to remove the auto-generated `--help`)', src/builder/debug_asserts.rs:121:17
```

---

_Label `C-bug` added by @alexheretic on 2022-09-28 18:46_

---

_Referenced in [alexheretic/ab-av1#52](../../alexheretic/ab-av1/pulls/52.md) on 2022-09-28 18:46_

---

_Referenced in [clap-rs/clap#4277](../../clap-rs/clap/pulls/4277.md) on 2022-09-28 19:35_

---

_Closed by @epage on 2022-09-28 19:45_

---

_Comment by @alexheretic on 2022-09-28 21:11_

Changing the example doesn't fix the, i thought similar, regression affecting https://github.com/alexheretic/ab-av1/pull/52 though.

Shall i raise another issue?

---

_Comment by @epage on 2022-09-28 21:25_

The panic is expected behavior; I just missed updating the example due to limited testing of it.

While its covered in the changelog, I go into more background on my [announcement](https://epage.github.io/blog/2022/09/clap4/#removing-implicit-version-help-behavior)

---
