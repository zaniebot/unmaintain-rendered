---
number: 4451
title: Usage shows erroneous message
type: issue
state: closed
author: hectorruiz-it
labels:
  - C-bug
assignees: []
created_at: 2022-11-04T23:43:35Z
updated_at: 2022-11-05T01:26:43Z
url: https://github.com/clap-rs/clap/issues/4451
synced_at: 2026-01-10T01:27:56Z
---

# Usage shows erroneous message

---

_Issue opened by @hectorruiz-it on 2022-11-04 23:43_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.63.0 (4b91a6ea7 2022-08-08)

### Clap Version

"4.0.18"

### Minimal reproducible code

```rust
use std::process::exit;

use clap::{Parser, Subcommand};
use clap::ArgAction::SetTrue;

mod list;
mod new;
mod setup;

#[derive(Parser)]
#[clap(about, version, author)]
struct Value {
    #[clap(subcommand)]
    command: Commands,
}

#[derive(Subcommand)]
enum Commands {
    /// List client platforms and repositories
    List {
        #[clap(long = "ssh", action = SetTrue, exclusive = true, required_unless_present_any = ["clients"])]
        /// List all platform ssh key alias
        ssh: bool,

        #[clap(long = "clients", action = SetTrue, exclusive = true, required_unless_present_any = ["ssh"])]
        /// List all clients)]
        clients: bool,
    }

}

fn main() {
    let value = Value::parse();
    match &value.command {
        Commands::List {
            ssh,
            clients,
        } => {
            if ssh.to_string() == "true" {
                match list::platforms() {
                    Ok(_) => exit(0),
                    Err(err) => {
                        eprintln!("ERROR: {}", err);
                        exit(1);
                    }
                };
            }
            if clients.to_string() == "true" {
                match list::clients() {
                    Ok(_) => exit(0),
                    Err(err) => {
                        eprintln!("ERROR: {}", err);
                        exit(1);
                    }
                }
            }
        }
    }
}


```


### Steps to reproduce the bug with the above code

cargo run list

### Actual Behaviour
```rust
error: The following required arguments were not provided:
  --ssh
  --clients

Usage: grabber list --ssh --clients

For more information try '--help'
```
### Expected Behaviour
```rust
error: One of the following required arguments were not provided:
  --ssh
  --clients

Usage: grabber list --ssh  || grabber list --clients

For more information try '--help'
```

### Additional Context

With the exclusive method, `--ssh` and `--clients` arguments can't be used together. The usage message doesn't representate this behaviour. 
If you run the usage message works as expected printing that it can't be used with one or more of the other specified arguments:
```rust
error: The argument '--ssh' cannot be used with one or more of the other specified arguments

Usage: grabber list [OPTIONS]

For more information try '--help'

```


### Debug Output

_No response_

---

_Label `C-bug` added by @hectorruiz-it on 2022-11-04 23:43_

---

_Comment by @epage on 2022-11-05 01:25_

imo this is a fairly complicated state for clap to infer that we are unlikely to support it.

However, another way of expressing this is by putting the two arguments into an `ArgGroup` and marking the `ArgGroup` as required.

Something like
```rust
use std::process::exit;

use clap::{Parser, Subcommand};
use clap::ArgAction::SetTrue;

mod list;
mod new;
mod setup;

#[derive(Parser)]
#[clap(about, version, author)]
struct Value {
    #[clap(subcommand)]
    command: Commands,
}

#[derive(Subcommand)]
enum Commands {
    /// List client platforms and repositories
    #[command(group = ArgGroup::new("action").required(true))]
    List {
        #[clap(group = "action")]
        /// List all platform ssh key alias
        ssh: bool,

        #[clap(group = "action")]
        /// List all clients)]
        clients: bool,
    }

}

fn main() {
    let value = Value::parse();
    match &value.command {
        Commands::List {
            ssh,
            clients,
        } => {
            if ssh.to_string() == "true" {
                match list::platforms() {
                    Ok(_) => exit(0),
                    Err(err) => {
                        eprintln!("ERROR: {}", err);
                        exit(1);
                    }
                };
            }
            if clients.to_string() == "true" {
                match list::clients() {
                    Ok(_) => exit(0),
                    Err(err) => {
                        eprintln!("ERROR: {}", err);
                        exit(1);
                    }
                }
            }
        }
    }
}
```

https://github.com/clap-rs/clap/issues/2621 will make this even easier

Side notes
- `long` can be inferred from the field name
- `bool` fields automatically get `ArgAction::SetTrue`
- exclusive should automatically disable `required = true`

---

_Comment by @epage on 2022-11-05 01:26_

Due to the complexity of supporting the inferring of this while we provide a more direct way for users to request this, I'm going to go ahead and close this.  If there is something I missed, we can look into re-opening this.

---

_Closed by @epage on 2022-11-05 01:26_

---
