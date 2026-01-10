---
number: 5255
title: "clap_complete: subcommands are broken in bash if the binary name is not the same as the package name"
type: issue
state: closed
author: Serock3
labels:
  - C-bug
assignees: []
created_at: 2023-12-13T16:46:11Z
updated_at: 2024-01-19T16:45:30Z
url: https://github.com/clap-rs/clap/issues/5255
synced_at: 2026-01-10T01:28:09Z
---

# clap_complete: subcommands are broken in bash if the binary name is not the same as the package name

---

_Issue opened by @Serock3 on 2023-12-13 16:46_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc -V

### Clap Version

4.4.11

### Minimal reproducible code

Cargo.toml
```toml
[package]
name = "package-name"
version = "0.1.0"
edition = "2021"

[[bin]]
name = "bin-name"
path = "src/main.rs"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
clap = { version = "4.4.11", features = ["derive"] }
clap_complete = { version = "4.4.4"}
```
main.rs
```rust
use clap::{Parser, Subcommand};
use clap_complete::Shell;

pub const BIN_NAME: &str = env!("CARGO_BIN_NAME");

#[derive(Debug, Parser)]
enum Cli {
    #[clap(subcommand)]
    Command(MySubcommand),
    ShellCompletions,
}

#[derive(Subcommand, Debug)]
enum MySubcommand {
    Get,
    Set,
}

fn main() {
    match Cli::parse() {
        Cli::Command(sub) => match sub {
            MySubcommand::Get => println!("get"),
            MySubcommand::Set => println!("set"),
        },
        Cli::ShellCompletions => {
            println!("Generating completions");
            use clap::CommandFactory;

            clap_complete::generate_to(Shell::Bash, &mut Cli::command(), BIN_NAME, "./")
                .unwrap();
        }
    }
}

```


### Steps to reproduce the bug with the above code

```bash
cargo run -- shell-completions

# Install completions
sudo cp bin-name.bash /usr/share/bash-completion/completions/bin-name
sudo cp target/debug/bin-name /usr/bin/
sudo chmod +x /usr/bin/bin-name
```

There will be no completions for the `get` or `set` subcommands when typing `bin-name command` and pressing tab.

### Actual Behaviour

The completion file will refer to the package name for subcommands.  Completions for top level commands will still use the binary name, hence the user will only get top level completions.

```bash
...
    for i in ${COMP_WORDS[@]}
    do
        case "${cmd},${i}" in
            ",$1")
                cmd="bin__name"
                ;;
            package__name,command)
                cmd="package__name__command"
                ;;
            package__name,help)
                cmd="package__name__help"
                ;;
            package__name,shell-completions)
                cmd="package__name__shell__completions"
                ;;
            package__name__command,get)
                cmd="package__name__command__get"
...
```

### Expected Behaviour

The correct completion file looks like this:

```bash
...
    for i in ${COMP_WORDS[@]}
    do
        case "${cmd},${i}" in
            ",$1")
                cmd="bin__name"
                ;;
            bin__name,command)
                cmd="bin__name__command"
                ;;
            bin__name,help)
                cmd="bin__name__help"
                ;;
            bin__name,shell-completions)
                cmd="bin__name__shell__completions"
                ;;
            bin__name__command,get)
                cmd="bin__name__command__get"
...
```

Installing that file will generate completions for e.g. `bin-name command get` .

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @Serock3 on 2023-12-13 16:46_

---

_Referenced in [clap-rs/clap#5256](../../clap-rs/clap/pulls/5256.md) on 2023-12-13 16:46_

---

_Renamed from "Completions for subcommands are broken in bash if the binary name is not the same as the package name" to "clap_complete: subcommands are broken in bash if the binary name is not the same as the package name" by @Serock3 on 2023-12-14 08:17_

---

_Closed by @epage on 2024-01-19 16:45_

---

_Referenced in [mullvad/mullvadvpn-app#5710](../../mullvad/mullvadvpn-app/pulls/5710.md) on 2024-01-22 09:26_

---
