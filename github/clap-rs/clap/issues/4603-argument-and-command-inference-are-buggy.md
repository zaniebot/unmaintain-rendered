---
number: 4603
title: Argument and command inference are buggy
type: issue
state: closed
author: SUPERCILEX
labels: []
assignees: []
created_at: 2023-01-04T02:00:13Z
updated_at: 2023-03-29T04:36:17Z
url: https://github.com/clap-rs/clap/issues/4603
synced_at: 2026-01-07T13:12:20-06:00
---

# Argument and command inference are buggy

---

_Issue opened by @SUPERCILEX on 2023-01-04 02:00_

- Long argument inference randomly picks an argument on conflicts (https://github.com/clap-rs/clap/pull/4584).
- Command name inference fails with conflicting aliases (https://github.com/clap-rs/clap/pull/4583).

Note that the other way around works: long args handle conflicting aliases and command names reject non-aliasing conflicts.

---

_Comment by @epage on 2023-01-04 20:18_

Please fill out the bug report template, including a code sample and reproduction steps.

---

_Comment by @SUPERCILEX on 2023-01-04 20:29_

This is just a tracking issue so people know these bugs exist. As I said, I'm not going to waste my time on that.

---

_Comment by @epage on 2023-01-04 21:27_

Except you are wasting your time further because I don't have time to clean up after this.  Without reproduction steps, this is being closed.

---

_Closed by @epage on 2023-01-04 21:27_

---

_Comment by @SUPERCILEX on 2023-03-29 03:25_

```rust
#!/usr/bin/env rust-script
//! ```cargo
//! [dependencies]
//! clap = { version = "4.2.0", features = ["derive"] }
//! ```

use clap::*;

#[derive(Parser, Debug)]
#[command(infer_subcommands = true, infer_long_args = true)]
struct Infer {
    // Try ./clap.rs --a delete. This should fail since there's ambiguity.
    #[arg(long, alias = "abc")]
    first: bool,
    #[arg(long, alias = "aaa")]
    second: bool,

    #[command(subcommand)]
    cmd: Cmd,
}

#[derive(Subcommand, Debug)]
enum Cmd {
    // Try ./clap.rs d. This should work since the ambiguity is within the command.
    #[command(alias = "destroy")]
    Delete
}

fn main() {
    dbg!(Infer::parse());
}
```

---
