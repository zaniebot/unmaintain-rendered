---
number: 5103
title: "`clap_complete` fails of some optional arguments are missing"
type: issue
state: closed
author: janstarke
labels:
  - C-bug
assignees: []
created_at: 2023-08-30T09:05:53Z
updated_at: 2023-09-08T10:15:50Z
url: https://github.com/clap-rs/clap/issues/5103
synced_at: 2026-01-10T01:28:07Z
---

# `clap_complete` fails of some optional arguments are missing

---

_Issue opened by @janstarke on 2023-08-30 09:05_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.71.0 (8ede3aae2 2023-07-12)

### Clap Version

4.4.0

### Minimal reproducible code

## Missing `bin_name` argument
```rust
use clap::{CommandFactory, Parser};
use clap_complete::{Generator, Shell};

#[derive(Parser)]
#[clap()]
struct Cli {
    #[clap(long, default_value_t = false, num_args = 0)]
    sample_flag: bool,

    #[clap(long, num_args = 1)]
    autocomplete: Option<Shell>,
}

fn main() {
    let cli = Cli::parse();
    if let Some(shell) = cli.autocomplete {
        shell.generate(&Cli::command(), &mut std::io::stdout());
    }
}

#[cfg(test)]
mod tests {
    use assert_cmd::Command;

    #[test]
    fn test_without_bin_name() {
        let mut cmd = Command::cargo_bin(env!("CARGO_BIN_NAME")).unwrap();
        cmd.arg("--autocomplete").arg("bash").assert().success();
    }
}
```

## Missing `num_args` argument

```rust
use clap::{CommandFactory, Parser};
use clap_complete::{Generator, Shell};

#[derive(Parser)]
#[clap(bin_name=env!("CARGO_BIN_NAME"))]
struct Cli {
    #[clap(long, default_value_t = false)]
    sample_flag: bool,

    #[clap(long)]
    autocomplete: Option<Shell>,
}

fn main() {
    let cli = Cli::parse();
    if let Some(shell) = cli.autocomplete {
        shell.generate(&Cli::command(), &mut std::io::stdout());
    }
}

#[cfg(test)]
mod tests {
    use assert_cmd::Command;

    #[test]
    fn test_without_num_args() {
        let mut cmd = Command::cargo_bin(env!("CARGO_BIN_NAME")).unwrap();
        cmd.arg("--autocomplete").arg("bash").assert().success();
    }
}
```

### Steps to reproduce the bug with the above code

```
cargo test
```

### Actual Behaviour

There are a lot of `clap` arguments which are optional, such as `bin_name` or `num_args`. When I do not specify such arguments, `clap_complete` fails.

### Expected Behaviour

One of the following should be the case instead:

 - compilation should fail if arguments required for `clap_complete` are missing
 - reasonable default could be used
 - It should be documented, which arguments are required for `clap_complete` to work correctly

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @janstarke on 2023-08-30 09:05_

---

_Comment by @epage on 2023-08-30 18:30_

Normally I would ask that you write single-purpose issues but I suspect these are related.

It is not generally expected to run `shell.generate` directly as that will be missing several important steps.  In the [documentation](https://docs.rs/clap_complete/latest/clap_complete/), we encourage using `clap_complete::generate` and `clap_complete::generate_to`

---

_Comment by @janstarke on 2023-09-08 10:15_

Thank you, this really makes a difference :+1: 

---

_Closed by @janstarke on 2023-09-08 10:15_

---
