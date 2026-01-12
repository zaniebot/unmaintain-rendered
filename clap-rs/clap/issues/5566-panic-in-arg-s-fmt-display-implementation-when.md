```yaml
number: 5566
title: "Panic in `Arg`'s `fmt::Display` implementation when `num_args` is not explicitly set"
type: issue
state: closed
author: chenxiaolong
labels:
  - C-bug
assignees: []
created_at: 2024-07-05T20:00:15Z
updated_at: 2024-07-05T21:17:09Z
url: https://github.com/clap-rs/clap/issues/5566
synced_at: 2026-01-12T16:14:17Z
```

# Panic in `Arg`'s `fmt::Display` implementation when `num_args` is not explicitly set

---

_@chenxiaolong_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.79.0 (129f3b996 2024-06-10) (Fedora 1.79.0-3.fc40)

### Clap Version

4.5.8

### Minimal reproducible code

```rust
use clap::{CommandFactory, Parser};

#[derive(Debug, Parser)]
struct Cli {
    #[clap(long, num_args = 1)]
    does_not_panic: String,

    #[clap(long)]
    does_panic: String,
}

fn main() {
    let command = Cli::command();

    for arg in command.get_arguments() {
        println!("{arg}");
    }
}
```


### Steps to reproduce the bug with the above code

```
RUST_BACKTRACE=1 cargo run
```

### Actual Behaviour

```
     Running `target/debug/clap-panic`
--does-not-panic <DOES_NOT_PANIC>
thread 'main' panicked at /home/chenxiaolong/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.8/src/builder/arg.rs:3986:29:
Fatal internal error. Please consider filing a bug report at https://github.com/clap-rs/clap/issues
stack backtrace:
   0: rust_begin_unwind
             at /builddir/build/BUILD/rustc-1.79.0-src/library/std/src/panicking.rs:652:5
   1: core::panicking::panic_fmt
             at /builddir/build/BUILD/rustc-1.79.0-src/library/core/src/panicking.rs:72:14
   2: core::panicking::panic_display
             at /builddir/build/BUILD/rustc-1.79.0-src/library/core/src/panicking.rs:263:5
   3: core::option::expect_failed
             at /builddir/build/BUILD/rustc-1.79.0-src/library/core/src/option.rs:1994:5
   4: core::option::Option<T>::expect
             at /builddir/build/BUILD/rustc-1.79.0-src/library/core/src/option.rs:895:21
   5: clap_builder::builder::arg::Arg::get_min_vals
             at /home/chenxiaolong/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.8/src/builder/arg.rs:3986:9
   6: clap_builder::builder::arg::Arg::stylize_arg_suffix
             at /home/chenxiaolong/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.8/src/builder/arg.rs:4309:35
   7: clap_builder::builder::arg::Arg::stylized
             at /home/chenxiaolong/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.8/src/builder/arg.rs:4297:29
   8: <clap_builder::builder::arg::Arg as core::fmt::Display>::fmt
             at /home/chenxiaolong/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.8/src/builder/arg.rs:4437:9
   9: <&T as core::fmt::Display>::fmt
             at /builddir/build/BUILD/rustc-1.79.0-src/library/core/src/fmt/mod.rs:2343:62
  10: core::fmt::rt::Argument::fmt
             at /builddir/build/BUILD/rustc-1.79.0-src/library/core/src/fmt/rt.rs:165:63
  11: core::fmt::write
             at /builddir/build/BUILD/rustc-1.79.0-src/library/core/src/fmt/mod.rs:1157:21
  12: std::io::Write::write_fmt
             at /builddir/build/BUILD/rustc-1.79.0-src/library/std/src/io/mod.rs:1832:15
  13: <&std::io::stdio::Stdout as std::io::Write>::write_fmt
             at /builddir/build/BUILD/rustc-1.79.0-src/library/std/src/io/stdio.rs:787:9
  14: <std::io::stdio::Stdout as std::io::Write>::write_fmt
             at /builddir/build/BUILD/rustc-1.79.0-src/library/std/src/io/stdio.rs:761:9
  15: std::io::stdio::print_to
             at /builddir/build/BUILD/rustc-1.79.0-src/library/std/src/io/stdio.rs:1117:21
  16: std::io::stdio::_print
             at /builddir/build/BUILD/rustc-1.79.0-src/library/std/src/io/stdio.rs:1194:5
  17: clap_panic::main
             at ./src/main.rs:16:9
  18: core::ops::function::FnOnce::call_once
             at /builddir/build/BUILD/rustc-1.79.0-src/library/core/src/ops/function.rs:250:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

### Expected Behaviour

The `fmt::Display` implementation shouldn't panic.

### Additional Context

This is the simplest reproducer I could find. My actual goal is being able to obtain the `--arg <ARG>` string for printing custom errors.

### Debug Output

This produces no additional output because the example is only looking at `Command::get_arguments()` and does not actually run the parser.

---

_Label `C-bug` added by @chenxiaolong on 2024-07-05 20:00_

---

_Comment by @epage on 2024-07-05 20:51_

In this case, its because the args need to be built first (`command.build()`).  Is there a reason that they are being printed without being built?

---

_Comment by @chenxiaolong on 2024-07-05 21:17_

Thanks! I completely overlooked that `Command::build()` existed. Looking at the docs for that function, it sounds like this behavior is intended, so I'll close out the issue. Sorry for the noise!

---

_Closed by @chenxiaolong on 2024-07-05 21:17_

---

_Referenced in [hermit-os/uhyve#1048](../../hermit-os/uhyve/pulls/1048.md) on 2025-08-08 00:12_

---
