---
number: 5592
title: Disable generating arguments after double hyphen for nushell
type: issue
state: open
author: ndtoan96
labels:
  - C-bug
  - E-medium
  - A-completion
assignees: []
created_at: 2024-07-21T20:10:25Z
updated_at: 2024-07-22T15:04:29Z
url: https://github.com/clap-rs/clap/issues/5592
synced_at: 2026-01-07T13:12:20-06:00
---

# Disable generating arguments after double hyphen for nushell

---

_Issue opened by @ndtoan96 on 2024-07-21 20:10_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.78.0 (9b00956e5 2024-04-29)

### Clap Version

4.3.5

### Minimal reproducible code

```rust
use std::{ffi::OsString, io, path::PathBuf};

use clap::{CommandFactory, Parser};
use clap_complete::generate;
use clap_complete_nushell::Nushell;

#[derive(Parser, Debug)]
pub struct RuffArgs {
    /// List of files or directories to limit the operation to
    paths: Vec<PathBuf>,
    /// Perform the operation on all packages
    #[arg(short, long)]
    all: bool,
    /// Perform the operation on a specific package
    #[arg(short, long)]
    package: Vec<String>,
    /// Use this pyproject.toml file
    #[arg(long, value_name = "PYPROJECT_TOML")]
    pyproject: Option<PathBuf>,
    /// Enables verbose diagnostics.
    #[arg(short, long)]
    verbose: bool,
    /// Turns off all output.
    #[arg(short, long, conflicts_with = "verbose")]
    quiet: bool,
    /// Extra arguments to ruff
    #[arg(last = true)]
    extra_args: Vec<OsString>,
    
}

fn main() {
    let mut cmd = RuffArgs::command();
    generate(Nushell,  &mut cmd,"myapp", &mut io::stdout());
}

```


### Steps to reproduce the bug with the above code

1. cargo run
2. copy the code to nushell env.nu
3. reload nushell
4. see error

### Actual Behaviour

```
Error: nu::parser::multiple_rest_params

  × Multiple rest params.
    ╭─[D:\dev_env\conf\nushell\scripts\rye.nu:85:28]
 84 │       # Run the code formatter on the project
 85 │ ╭─▶   export extern "rye fmt" [
 86 │ │       ...paths: string          # List of files or directories to limit the operation to
 87 │ │       --all(-a)                 # Perform the operation on all packages
 88 │ │       --package(-p): string     # Perform the operation on a specific package
 89 │ │       --pyproject: string       # Use this pyproject.toml file
 90 │ │       --verbose(-v)             # Enables verbose diagnostics
 91 │ │       --quiet(-q)               # Turns off all output
 92 │ │       ...extra_args: string     # Extra arguments to ruff
 93 │ │       --check                   # Run format in check mode
 94 │ │       --help(-h)                # Print help (see more with '--help')
 95 │ ├─▶   ]
    · ╰──── multiple rest params
 96 │
    ╰────
```

### Expected Behaviour

Completion should not cause error in nushell.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @ndtoan96 on 2024-07-21 20:10_

---

_Referenced in [astral-sh/rye#1030](../../astral-sh/rye/pulls/1030.md) on 2024-07-21 20:11_

---

_Label `E-medium` added by @epage on 2024-07-22 15:02_

---

_Label `A-completion` added by @epage on 2024-07-22 15:02_

---

_Comment by @epage on 2024-07-22 15:04_

Ideally in fixing this, someone would write integration tests with completest-nu

Please add the tests in the first commit with them passing, showing the current bug.  The following commit would fix the behavior and update the test so it still passes.

---
