```yaml
number: 5300
title: "behavior change regarding `possible_values` on derived boolean args in 4.4.14"
type: issue
state: closed
author: ahl
labels:
  - C-bug
assignees: []
created_at: 2024-01-12T16:56:30Z
updated_at: 2024-01-12T19:34:19Z
url: https://github.com/clap-rs/clap/issues/5300
synced_at: 2026-01-12T16:14:16Z
```

# behavior change regarding `possible_values` on derived boolean args in 4.4.14

---

_@ahl_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.75.0 (82e1608df 2023-12-21)

### Clap Version

4.4.14

### Minimal reproducible code

```rust
use clap::{CommandFactory, Parser};

#[derive(Parser)]
pub struct Args {
    #[clap(long)]
    ok: bool,
}

fn main() {
    let values = Args::command()
        .get_arguments()
        .next()
        .expect("no arguments")
        .get_possible_values();
    println!("{:#?}", values);
}
```

### Steps to reproduce the bug with the above code

Use 4.4.13; cargo run:
```
[]
```

Use 4.4.14; cargo run:
```
[
    PossibleValue {
        name: "true",
        help: None,
        aliases: [],
        hide: false,
    },
    PossibleValue {
        name: "false",
        help: None,
        aliases: [],
        hide: false,
    },
]
```


### Actual Behaviour

Behavior change from 4.4.13 to 4.4.14

### Expected Behaviour

No behavior change? Unless this was intentional. I didn't see it mentioned in the release notes.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @ahl on 2024-01-12 16:56_

---

_Referenced in [oxidecomputer/oxide.rs#513](../../oxidecomputer/oxide.rs/pulls/513.md) on 2024-01-12 16:56_

---

_Comment by @epage on 2024-01-12 18:29_

This change of behavior is expected but there is something you can do on your side to address it.

In clap, args have two settings that describe whether a value is accepted or not
- `num_args`
- `action`

When an argument doesn't take a value, we don't return the possible values.

With 4.4.14, we added support for `.action(ArgAction::Set).num_args(0)` which introduced a discrepancy, `action` only describe if an arg *might* take a value.

To do this, we needed to change out "takes value" tracking which is mostly unnoticeable because we auto-fill in properties when parsing.

The problem is that you are looking at the possible values without the auto-fill of properties.  **If you call `Command::build`, the problem goes away.**

See
```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { path = "../clap", features = ["derive"] }
---

use clap::{CommandFactory, Parser};

#[derive(Parser)]
pub struct Args {
    #[clap(long)]
    ok: bool,
}

fn main() {
    let mut cmd = Args::command();
    cmd.build();
    let values = cmd
        .get_arguments()
        .next()
        .expect("no arguments")
        .get_possible_values();
    println!("{:#?}", values);
}
```

---

_Comment by @epage on 2024-01-12 18:30_

#2911 covers some of this problem in the API

---

_Comment by @ahl on 2024-01-12 19:05_

This is extremely helpful. Thanks very much. Feel free to close this, or leave it open if there's some work item such as updating the release notes that might be relevant.

---

_Referenced in [oxidecomputer/oxide.rs#519](../../oxidecomputer/oxide.rs/pulls/519.md) on 2024-01-12 19:20_

---

_Closed by @epage on 2024-01-12 19:34_

---
