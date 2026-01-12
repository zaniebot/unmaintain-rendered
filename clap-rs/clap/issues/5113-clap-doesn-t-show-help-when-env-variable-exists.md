```yaml
number: 5113
title: "Clap doesn't show help when env variable exists"
type: issue
state: closed
author: coltonhurst
labels:
  - C-bug
assignees: []
created_at: 2023-09-06T15:56:14Z
updated_at: 2023-09-06T16:05:29Z
url: https://github.com/clap-rs/clap/issues/5113
synced_at: 2026-01-12T16:14:16Z
```

# Clap doesn't show help when env variable exists

---

_@coltonhurst_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.71.1

### Clap Version

4.4.2

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Parser)]
#[command(author, version, about, long_about = None)]
#[command(propagate_version = true)]
struct Cli {
    #[command(subcommand)]
    command: Command,

    #[arg(short = 't', long, global = true, env = "ACCESS_TOKEN")]
    access_token: Option<String>,
}

#[derive(Subcommand)]
enum Command {
    #[command(long_about = "Commands related to employees")]
    Employee {
        #[command(subcommand)]
        cmd: EmployeeCommand,
    },
    #[command(long_about = "Commands related to emails")]
    Email {
        #[command(subcommand)]
        cmd: EmailCommand,
    },
}

#[derive(Subcommand)]
enum EmployeeCommand {
    Get { id: u32 },
    Create { name: String },
}

#[derive(Subcommand)]
enum EmailCommand {
    List { id: u32 },
    Create { name: String },
}

fn main() {
    let cli = Cli::parse();

    match &cli.command {
        Command::Employee { cmd } => match cmd {
            EmployeeCommand::Create { name } => {
                println!("Create the employee with the name: {}", name);
            }
            EmployeeCommand::Get { id } => {
                println!("Get the employee with the id: {}", id);
            }
        },
        Command::Email { cmd } => match cmd {
            EmailCommand::Create { name } => {
                println!("Create an email with the name: {}", name);
            }
            EmailCommand::List { id } => {
                println!("List the email with the id: {}", id);
            }
        },
    }

    match &cli.access_token {
        Some(value) => println!("Access token provided: {}", value),
        None => println!("No access token provided"),
    }
}
```

### Steps to reproduce the bug with the above code

1. `cargo build`
2. `export ACCESS_TOKEN="12345"`
3. `./target/debug/clap-env-test`


### Actual Behaviour

When an environment variable is set that clap expects, help text is not printed when no options are passed.

### Expected Behaviour

Help text should be printed when options are not passed. This works, except when an environment variable that clap expects is defined. Despite any environment variables set, the help text should still print when no options are provided.

### Additional Context

Credit to [tangowithfoxtrot](https://github.com/tangowithfoxtrot) for discovering the bug.

In case it's helpful, I have created a repository around the example provided above with more context and detail here: [https://github.com/coltonhurst/clap-env-test](https://github.com/coltonhurst/clap-env-test)

### Debug Output

_No response_

---

_Label `C-bug` added by @coltonhurst on 2023-09-06 15:56_

---

_Comment by @epage on 2023-09-06 16:05_

Closing in favor of #3572

---

_Closed by @epage on 2023-09-06 16:05_

---

_Referenced in [clap-rs/clap#5673](../../clap-rs/clap/issues/5673.md) on 2024-08-14 05:16_

---
