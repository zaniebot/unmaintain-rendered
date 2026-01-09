---
number: 5607
title: Subcommand about text is incorrectly wrapped
type: issue
state: closed
author: noc7c9
labels:
  - C-bug
assignees: []
created_at: 2024-07-30T02:15:42Z
updated_at: 2024-07-31T21:24:11Z
url: https://github.com/clap-rs/clap/issues/5607
synced_at: 2026-01-07T13:12:20-06:00
---

# Subcommand about text is incorrectly wrapped

---

_Issue opened by @noc7c9 on 2024-07-30 02:15_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.80.0 (051478957 2024-07-21)

### Clap Version

4.5.11

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Debug, Parser)]
struct Args {
    #[clap(subcommand)]
    sub: Command,
}

#[derive(Debug, Subcommand)]
enum Command {
    /// About text for subcommand that's just a little bit too long and so needs to be wrapped
    CmdName,
}

fn main() {
    Args::parse();
}
```

### Steps to reproduce the bug with the above code

`cargo run -- -h` on a narrow enough terminal (examples below are 60 cols).

### Actual Behaviour

```
Commands:
  cmd-name  About text for subcommand that's just a
                little bit too long and so needs to be
                wrapped
  help      Print this message or the help of the given
                subcommand(s)
```

### Expected Behaviour

```
Commands:
  cmd-name  About text for subcommand that's just a
            little bit too long and so needs to be
            wrapped
  help      Print this message or the help of the given
            subcommand(s)
```

### Additional Context

It works fine when you have a longer subcommand name.
```
Commands:
  a-much-longer-cmd-name
          About text for subcommand that's just a little bit
          too long and so needs to be wrapped
  help
          Print this message or the help of the given
          subcommand(s)
```

### Debug Output

_No response_

---

_Label `C-bug` added by @noc7c9 on 2024-07-30 02:15_

---

_Renamed from "Command about text is incorrectly wrapped" to "Subcommand about text is incorrectly wrapped" by @noc7c9 on 2024-07-30 02:54_

---

_Referenced in [clap-rs/clap#5615](../../clap-rs/clap/pulls/5615.md) on 2024-07-31 21:18_

---

_Closed by @epage on 2024-07-31 21:24_

---
