```yaml
number: 3883
title: Clap-complete example code not working
type: issue
state: closed
author: wwood
labels:
  - C-bug
assignees: []
created_at: 2022-06-29T00:56:56Z
updated_at: 2022-07-11T21:22:58Z
url: https://github.com/clap-rs/clap/issues/3883
synced_at: 2026-01-12T16:14:15Z
```

# Clap-complete example code not working

---

_@wwood_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.60.0 (7737e0b5c 2022-04-04)

### Clap Version

clap 3.2.7, clap_complete 3.2.3

### Minimal reproducible code

```rust

// Code copied directly from https://docs.rs/clap_complete/latest/clap_complete/

use clap::{Command, AppSettings, Arg, ValueHint, value_parser};
use clap_complete::{generate, Generator, Shell};
use std::io;

fn build_cli() -> Command<'static> {
    Command::new("example")
         .arg(Arg::new("file")
             .help("some input file")
                .value_hint(ValueHint::AnyPath),
        )
       .arg(
           Arg::new("generator")
               .long("generate")
               .value_parser(value_parser!(Shell)),
       )
}

fn print_completions<G: Generator>(gen: G, cmd: &mut Command) {
    generate(gen, cmd, cmd.get_name().to_string(), &mut io::stdout());
}

fn main() {
    let matches = build_cli().get_matches();

    if let Ok(generator) = matches.value_of_t::<Shell>("generator") {
        let mut cmd = build_cli();
        eprintln!("Generating completion file for {}...", generator);
        print_completions(generator, &mut cmd);
    }
}
```


### Steps to reproduce the bug with the above code

$ cargo run -- --generate bash
   Compiling clap_complete_debug v0.1.0 (/home/ben/git/clap_complete_debug)
    Finished dev [unoptimized + debuginfo] target(s) in 0.58s
     Running `target/debug/clap_complete_debug --generate bash`
thread 'main' panicked at 'Must use `_os` lookups with `Arg::allow_invalid_utf8` at `generator`', src/main.rs:26:30
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

### Actual Behaviour

It errors as seen above.

### Expected Behaviour

The shell completions should be written to stdout.

### Additional Context

importing `AppSettings` is also unneeded so generates a warning.

Thanks in advance.

### Debug Output

_No response_

---

_Label `C-bug` added by @wwood on 2022-06-29 00:56_

---

_Referenced in [Homebrew/homebrew-core#104819](../../Homebrew/homebrew-core/pulls/104819.md) on 2022-07-03 15:44_

---

_Referenced in [clap-rs/clap#3906](../../clap-rs/clap/pulls/3906.md) on 2022-07-11 20:20_

---

_Closed by @epage on 2022-07-11 20:36_

---

_Comment by @wwood on 2022-07-11 21:22_

Thanks @epage - works for me now.

---
