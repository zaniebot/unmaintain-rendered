---
number: 3888
title: Duplicating a subcommand name as a subcommand alias is not prevented
type: issue
state: closed
author: epage
labels:
  - C-bug
  - M-breaking-change
  - A-builder
  - E-easy
assignees: []
created_at: 2022-06-30T13:20:43Z
updated_at: 2022-07-25T19:18:11Z
url: https://github.com/clap-rs/clap/issues/3888
synced_at: 2026-01-07T13:12:20-06:00
---

# Duplicating a subcommand name as a subcommand alias is not prevented

---

_Issue opened by @epage on 2022-06-30 13:20_

The following should panic in a debug assert but doesn't.  It should mirror the asserts for duplicate flags
```rust
#!/usr/bin/env -S rust-script --debug

//! ```cargo
//! [dependencies]
//! clap = { path = "../clap", features = ["env", "derive"] }
//! ```

fn main() {
    let mut cmd = clap::Command::new("myprog")
        .subcommand(clap::Command::new("foo"))
        .subcommand(clap::Command::new("bar").alias("foo"));
    cmd.build();
}
```

---

_Label `C-bug` added by @epage on 2022-06-30 13:20_

---

_Label `M-breaking-change` added by @epage on 2022-06-30 13:20_

---

_Label `A-builder` added by @epage on 2022-06-30 13:20_

---

_Label `E-easy` added by @epage on 2022-06-30 13:20_

---

_Added to milestone `4.0` by @epage on 2022-06-30 13:20_

---

_Referenced in [clap-rs/clap#3882](../../clap-rs/clap/pulls/3882.md) on 2022-06-30 13:21_

---

_Referenced in [clap-rs/clap#3909](../../clap-rs/clap/pulls/3909.md) on 2022-07-12 01:58_

---

_Referenced in [clap-rs/clap#3989](../../clap-rs/clap/pulls/3989.md) on 2022-07-25 18:46_

---

_Closed by @epage on 2022-07-25 19:18_

---
