---
number: 3644
title: Improved fish shell completion with filename parameters
type: issue
state: closed
author: Icelk
labels:
  - C-bug
  - A-completion
  - E-easy
assignees: []
created_at: 2022-04-20T06:38:59Z
updated_at: 2024-08-10T00:45:00Z
url: https://github.com/clap-rs/clap/issues/3644
synced_at: 2026-01-07T13:12:19-06:00
---

# Improved fish shell completion with filename parameters

---

_Issue opened by @Icelk on 2022-04-20 06:38_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.60.0 (7737e0b5c 2022-04-04)

### Clap Version

3.1.10

### Minimal reproducible code

Any program which writes fish completions, has a file path as a unordered argument, and has subcommands.

```rust
use clap::{Command, AppSettings, Arg, ValueHint};
use clap_complete::{generate, Generator, Shell};
use std::io;

fn build_cli() -> Command<'static> {
    Command::new("example")
         .arg(Arg::new("file")
             .help("some input file")
             .required(true)
             .multiple(true)
                .value_hint(ValueHint::AnyPath),
        )
       .subcommand(
           Command::new("generator")
       )
}

fn print_completions<G: Generator>(gen: G, cmd: &mut Command) {
    generate(gen, cmd, cmd.get_name().to_string(), &mut io::stdout());
}

fn main() {
    let matches = build_cli().get_matches();

    if let Some(_) = matches.subcommand_matches("generator") {
        let mut cmd = build_cli();
        eprintln!("Generating completion file for {}...", generator);
        print_completions(Shell::Fish, &mut cmd);
    }
}
```

### Steps to reproduce the bug with the above code

`cargo run -- generate > ~/.config/fish/completions/<cargo project name>.fish`

### Actual Behaviour

When trying to complete now, files aren't completed as you'd expect. Fish expects the first argument to be a subcommand, but it can also be a file.

### Expected Behaviour

Fish should complete both subcommands and files.

This was solved by prepending the following to the top of the completion file, where `chute` is my binary name:

```
complete -c chute -F
```

This could be done if a file path argument exists and we have subcommands.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @Icelk on 2022-04-20 06:38_

---

_Label `A-completion` added by @epage on 2022-04-20 13:54_

---

_Label `E-easy` added by @epage on 2022-04-20 13:54_

---

_Comment by @epage on 2024-08-10 00:44_

As I believe this is resolved with the native completions, I'm going to close this.  If this was incorrect, let us know!

You can track stabilization at #3166

---

_Closed by @epage on 2024-08-10 00:44_

---
