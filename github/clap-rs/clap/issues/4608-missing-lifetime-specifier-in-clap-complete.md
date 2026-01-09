---
number: 4608
title: "Missing lifetime specifier in `clap_complete` example"
type: issue
state: closed
author: GJZwiers
labels:
  - C-bug
assignees: []
created_at: 2023-01-06T11:02:58Z
updated_at: 2023-01-06T12:57:31Z
url: https://github.com/clap-rs/clap/issues/4608
synced_at: 2026-01-07T13:12:20-06:00
---

# Missing lifetime specifier in `clap_complete` example

---

_Issue opened by @GJZwiers on 2023-01-06 11:02_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.65.0 (897e37553 2022-11-02)

### Clap Version

3.1.12

### Minimal reproducible code

```rust
use clap::{Command, Arg, ValueHint, value_parser, ArgAction};
use clap_complete::{generate, Generator, Shell};
use std::io;

fn build_cli() -> Command {
    Command::new("example")
         .arg(Arg::new("file")
             .help("some input file")
                .value_hint(ValueHint::AnyPath),
        )
       .arg(
           Arg::new("generator")
               .long("generate")
               .action(ArgAction::Set)
               .value_parser(value_parser!(Shell)),
       )
}

fn print_completions<G: Generator>(gen: G, cmd: &mut Command) {
    generate(gen, cmd, cmd.get_name().to_string(), &mut io::stdout());
}

fn main() {
    let matches = build_cli().get_matches();

    if let Some(generator) = matches.get_one::<Shell>("generator").copied() {
        let mut cmd = build_cli();
        eprintln!("Generating completion file for {}...", generator);
        print_completions(generator, &mut cmd);
    }
}
```

### Steps to reproduce the bug with the above code

- Add the code from https://docs.rs/clap_complete/latest/clap_complete/#example to a new Cargo project
- Run `cargo build`

### Actual Behaviour

```
cargo build
   Compiling clap_repro v0.1.0 (C:\Users\GJZwiers\repos\clap_repro)
error[E0106]: missing lifetime specifier
 --> src\main.rs:5:19
  |
5 | fn build_cli() -> Command {
  |                   ^^^^^^^ expected named lifetime parameter
  |
  = help: this function's return type contains a borrowed value, but there is no value for it to be borrowed from
help: consider using the `'static` lifetime
  |
5 | fn build_cli() -> Command<'static> {
  |                          +++++++++

For more information about this error, try `rustc --explain E0106`.
error: could not compile `clap_repro` due to previous error
```

### Expected Behaviour

The code example should be valid and build without errors.

### Additional Context

It can be solved by changing the return type of `build_cli` to `Command<'static>`.

### Debug Output

_No response_

---

_Label `C-bug` added by @GJZwiers on 2023-01-06 11:02_

---

_Comment by @epage on 2023-01-06 12:57_

The [example link](https://docs.rs/clap_complete/latest/clap_complete/#example) you gave is for clap_complete "latest"  which is currently 4.0.7 while you are using clap v3 and would need to use the version selector to select a [compatible version of clap_complete](https://docs.rs/clap_complete/3.2.5/clap_complete/index.html#example)

---

_Closed by @epage on 2023-01-06 12:57_

---
