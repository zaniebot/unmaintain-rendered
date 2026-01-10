---
number: 4479
title: "Printing an argument from `get_positionals()` panicks with \"Fatal internal error\""
type: issue
state: closed
author: mss1451
labels:
  - C-bug
assignees: []
created_at: 2022-11-13T12:12:01Z
updated_at: 2022-11-14T18:30:41Z
url: https://github.com/clap-rs/clap/issues/4479
synced_at: 2026-01-10T01:27:56Z
---

# Printing an argument from `get_positionals()` panicks with "Fatal internal error"

---

_Issue opened by @mss1451 on 2022-11-13 12:12_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.65.0 (897e37553 2022-11-02)

### Clap Version

4.0.23

### Minimal reproducible code

```rust
use clap::{Command, Arg};

fn main() {
    env_logger::init();
    let command = Command::new("App Name").arg(Arg::new("arg"));
    let positionals = command.get_positionals();
    for arg in positionals {
        println!("arg: {arg}");
    }
}
```

### Steps to reproduce the bug with the above code

Running `cargo run` is enough to trigger the fatal error.

### Actual Behaviour

Panics when it is about to print an argument.
```
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/clap-rs/clap/issues', /home/sami/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.23/src/builder/arg.rs:4186:44
stack backtrace:
   0: rust_begin_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:584:5
   1: core::panicking::panic_fmt
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panicking.rs:142:14
   2: core::panicking::panic_display
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panicking.rs:72:5
   3: core::panicking::panic_str
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panicking.rs:56:5
   4: core::option::expect_failed
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/option.rs:1880:5
   5: core::option::Option<T>::expect
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/option.rs:738:21
   6: clap::builder::arg::Arg::render_arg_val
             at /home/sami/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.23/src/builder/arg.rs:4186:24
   7: clap::builder::arg::Arg::stylize_arg_suffix
             at /home/sami/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.23/src/builder/arg.rs:4170:27
   8: clap::builder::arg::Arg::stylized
             at /home/sami/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.23/src/builder/arg.rs:4144:23
   9: <clap::builder::arg::Arg as core::fmt::Display>::fmt
             at /home/sami/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.23/src/builder/arg.rs:4264:9
  10: <&T as core::fmt::Display>::fmt
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/fmt/mod.rs:2366:62
  11: core::fmt::write
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/fmt/mod.rs:1202:17
  12: std::io::Write::write_fmt
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/io/mod.rs:1679:15
  13: <&std::io::stdio::Stdout as std::io::Write>::write_fmt
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/io/stdio.rs:715:9
  14: <std::io::stdio::Stdout as std::io::Write>::write_fmt
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/io/stdio.rs:689:9
  15: std::io::stdio::print_to
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/io/stdio.rs:1017:21
  16: std::io::stdio::_print
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/io/stdio.rs:1030:5
  17: auto_i18n::main
             at ./src/main.rs:7:9
  18: core::ops::function::FnOnce::call_once
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/ops/function.rs:248:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

### Expected Behaviour

Printed positional arguments. I don't know if the code makes sense or not as I don't quite know how clap works. I was merely experimenting.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @mss1451 on 2022-11-13 12:12_

---

_Referenced in [clap-rs/clap#4480](../../clap-rs/clap/pulls/4480.md) on 2022-11-14 18:13_

---

_Comment by @epage on 2022-11-14 18:14_

 Note for it to render a meaningful value, you'll need to call `Command::build`.

---

_Closed by @epage on 2022-11-14 18:30_

---
