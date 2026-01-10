---
number: 3508
title: "generated zsh completion fails when `source <()`'d"
type: issue
state: closed
author: drahnr
labels:
  - C-bug
  - A-completion
  - E-easy
assignees: []
created_at: 2022-02-25T19:25:43Z
updated_at: 2023-02-27T16:17:49Z
url: https://github.com/clap-rs/clap/issues/3508
synced_at: 2026-01-10T01:27:42Z
---

# generated zsh completion fails when `source <()`'d

---

_Issue opened by @drahnr on 2022-02-25 19:25_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.58.1 (db9d1b20b 2022-01-20)

### Clap Version

3.1.2

### Minimal reproducible code

```rust
use clap::{AppSettings, Arg, Command, PossibleValue, Subcommand, ValueHint};
use clap_complete::{generate, Generator, Shell};
use std::{io, collections::{HashSet, HashMap}};

fn build_cli() -> Command<'static> {
    Command::new(env!("CARGO_PKG_NAME"))
        .subcommand_required(true)
        .subcommand(
            Command::new("foo")
            .arg(
                Arg::new("foo")
                    .long("foo")
                    .help("foos for real")
                    .possible_value(PossibleValue::new("7"))
                    .possible_value(PossibleValue::new("seven"))
            ),
        )
        .subcommand(
            Command::new("comp")
            .arg(
                Arg::new("shell")
                    .long("shell")
                    .help("shell to generate for")
                    .possible_values(Shell::possible_values()),
            ),
        )
}

fn print_completions<G: Generator>(gen: G, cmd: &mut Command) {
    generate(gen, cmd, cmd.get_name().to_string(), &mut io::stdout());
}

fn main() {
    let app = build_cli();
    let matches = app.get_matches();

    match matches.subcommand() {
        Some(("comp", args)) => {
            if let Ok(shell) = args.value_of_t::<Shell>("shell") {
                let mut cmd = build_cli();
                eprintln!("Generating completion file for {}...", shell);
                print_completions(dbg!(shell), &mut cmd);
                return;
            }
            eprintln!("nope");
        }
        Some(("foo", _args)) => {
            println!("foo");
            return;
        }
        _ => {}
    }
    build_cli().print_long_help().unwrap();
}

```


### Steps to reproduce the bug with the above code

`source <(cargo run -- comp --shell zsh)`


### Actual Behaviour

errors out and the completion does not work with the binary afterwards

error:
```
# source <(cargo run -- comp --shell zsh)
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/clap-completion-zoink comp --shell zsh`
Generating completion file for zsh...
[src/main.rs:42] shell = Zsh

_arguments:comparguments:325: can only be called from completion function
```

### Expected Behaviour

should source the completion and from thereon complete the execution

### Additional Context

An _almost_ minimal, reproducible example can be checked out at https://github.com/drahnr/clap-completion-zoink

Encountered in the wild on https://github.com/soywod/himalaya and reported as https://github.com/volta-cli/volta/issues/496

### Debug Output

_No response_

---

_Label `C-bug` added by @drahnr on 2022-02-25 19:25_

---

_Label `A-completion` added by @epage on 2022-02-25 19:35_

---

_Label `E-easy` added by @epage on 2022-02-25 19:35_

---

_Comment by @jjmark15 on 2022-04-22 19:02_

did you find a way to avoid the issue?

---

_Comment by @drahnr on 2022-04-24 14:29_

The generated file is meant to be written to a file, assuming the above project is `foo`, `_foo` would be the correct file name which has to reside in `$fpath`.

There also seems to be an artifact when writing to a file via `foo comp --shell zsh > _foo`:

```
<snip>
_cargo-spellcheck "$@"

```

which doesn't seem to be an ordinary linebreak when openeing an editor :shrug: 
![grafik](https://user-images.githubusercontent.com/667047/164981502-ed7963f3-d565-4032-bcac-36c190c106be.png)

---

Quick fix:

Delete the comment `#compinit foo` marker at the top and the last line `_foo` and it then should load fine, unless you hit #3022


---

_Referenced in [kindelia/Kindelia#219](../../kindelia/Kindelia/pulls/219.md) on 2022-11-08 00:42_

---

_Referenced in [vrc-get/vrc-get#35](../../vrc-get/vrc-get/issues/35.md) on 2023-02-10 13:58_

---

_Referenced in [clap-rs/clap#4734](../../clap-rs/clap/pulls/4734.md) on 2023-02-27 09:42_

---

_Closed by @epage on 2023-02-27 16:17_

---
