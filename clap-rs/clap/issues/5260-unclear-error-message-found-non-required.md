```yaml
number: 5260
title: "Unclear error message: \"Found non-required positional argument with a lower index than ...\""
type: issue
state: open
author: RalphDNB
labels:
  - C-bug
assignees: []
created_at: 2023-12-19T21:15:29Z
updated_at: 2024-06-04T18:08:39Z
url: https://github.com/clap-rs/clap/issues/5260
synced_at: 2026-01-12T16:14:16Z
```

# Unclear error message: "Found non-required positional argument with a lower index than ..."

---

_@RalphDNB_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.74.1

### Clap Version

4.4.11

### Minimal reproducible code

```rust
#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
struct Args {
    /// The file you want to use
    ///
    /// Use `-` to read from stdin.
    #[arg(default_value = "-")]
    file: String,

    arg2: String,
}

fn main() {
    let args = Args::parse();
}
```

### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

thread 'main' panicked at /Users/user/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.11/src/builder/debug_asserts.rs:658:17:
Found non-required positional argument with a lower index than a required positional argument: "file" index Some(1)
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

### Expected Behaviour

A better clearer error message.
Maybe something like:
> The argument `arg2` has no default value, but `file` is a positional arguments, but has a default value.
> This results in a conflict where it is unknown to what variable the value belongs to.
> 
> Possible fix: 
> 1) Add a default value for `arg2` using `#[arg(default_value = "<some default value>")]`.
> 2) Remove the default value from `file`.
(ideally this would be highlighted like the rust compiler errors (but this might be more difficult))

### Additional Context

This error message is very difficult to understand. And it is hard to know what exactly goes wrong and how to fix it.

Error message in clap code: https://github.com/clap-rs/clap/blob/d092896d61fd73a5467db85eac035a9ce2ddbc60/clap_builder/src/builder/debug_asserts.rs#L660-L661

And similar: https://github.com/clap-rs/clap/blob/d092896d61fd73a5467db85eac035a9ce2ddbc60/clap_builder/src/builder/debug_asserts.rs#L630-L632

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="project-name-rs"
[clap_builder::builder::command]Command::_propagate:project-name-rs
[clap_builder::builder::command]Command::_check_help_and_version:project-name-rs expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_propagate_global_args:project-name-rs
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:file
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:arg2
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:version
[clap_builder::builder::debug_asserts]Command::_verify_positionals
thread 'main' panicked at /Users/<user_name>/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.11/src/builder/debug_asserts.rs:658:17:
Found non-required positional argument with a lower index than a required positional argument: "file" index Some(1)
stack backtrace:
   0: rust_begin_unwind
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:597:5
   1: core::panicking::panic_fmt
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/panicking.rs:72:14
   2: clap_builder::builder::debug_asserts::_verify_positionals
             at /Users/user/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.11/src/builder/debug_asserts.rs:658:17
   3: clap_builder::builder::debug_asserts::assert_app
             at /Users/user/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.11/src/builder/debug_asserts.rs:351:5
   4: clap_builder::builder::command::Command::_build_self
             at /Users/user/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.11/src/builder/command.rs:4123:13
   5: clap_builder::builder::command::Command::_do_parse
             at /Users/user/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.11/src/builder/command.rs:3994:9
   6: clap_builder::builder::command::Command::try_get_matches_from_mut
             at /Users/user/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.11/src/builder/command.rs:830:9
   7: clap_builder::builder::command::Command::get_matches_from
             at /Users/user/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.11/src/builder/command.rs:701:9
   8: clap_builder::builder::command::Command::get_matches
             at /Users/user/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.11/src/builder/command.rs:610:9
   9: clap_builder::derive::Parser::parse
             at /Users/user/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.4.11/src/derive.rs:27:27
  10: project_name_rs::main
             at ./src/main.rs:37:16
  11: core::ops::function::FnOnce::call_once
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/ops/function.rs:250:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

---

_Label `C-bug` added by @RalphDNB on 2023-12-19 21:15_

---

_Comment by @epage on 2023-12-19 21:22_

The complexity with this is the assert is independent of the derive API.  It would need to be worded to fit with both APIs.

---

_Comment by @lilygruman on 2024-04-11 02:07_

Just dropping a note in here for anyone else who runs into this:

The issue is that this error is about the **definition of the struct** in the source code, not the arguments the user did or didn't provide when running the program. Normally, this issue would be a **compiler** error, not a runtime panic, and the panic message doesn't hint at that at all.

Based on epage's reply, and the panic message itself coming from `clap-builder`, it seems some non-trivial work would need to be done to detect this at compile time instead of runtime, and doing so would decouple the APIs in an undesirable way.

---

_Comment by @epage on 2024-04-11 02:14_

> Normally, this issue would be a compiler error, not a runtime panic, and the panic message doesn't hint at that at all.

While compile time failures would be preferred, I think asserts should be communicating that its a development problems rather than a user problem.

Note that we do [encourage explicitly running `debug_asserts`](https://docs.rs/clap/latest/clap/_derive/_tutorial/chapter_4/index.html) which would also show that these are independent of the command-line.

---

_Comment by @lilygruman on 2024-04-11 02:31_

That's helpful to know. I'm sure I'm not alone in being caught off-guard by that approach, as I've never run into it before.

---

_Comment by @kaimast on 2024-06-04 18:08_

I also just ran into this and, for some reason, it was not clear to me that the error was on my side. The error message is fairly clear, but maybe it could be easier to understand if it included a link to this issue or some other page that explains how to resolve it? 

---
