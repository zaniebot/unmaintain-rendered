---
number: 5626
title: "docs: Arg::value_delimiter default value"
type: issue
state: closed
author: systemtrap
labels:
  - C-bug
assignees: []
created_at: 2024-08-05T21:20:15Z
updated_at: 2024-08-07T20:33:07Z
url: https://github.com/clap-rs/clap/issues/5626
synced_at: 2026-01-07T13:12:20-06:00
---

# docs: Arg::value_delimiter default value

---

_Issue opened by @systemtrap on 2024-08-05 21:20_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.80.0 (051478957 2024-07-21)

### Clap Version

4.5.13

### Minimal reproducible code

```rust
use clap::{Arg, Command};

fn main() {
    let m = Command::new("prog")
        .arg(Arg::new("config")
            .short('c')
            .long("config"))
        .get_matches_from(vec![
            "prog", "--config=val1,val2,val3"
        ]);

    assert_eq!(m.get_many::<String>("config").unwrap().collect::<Vec<_>>(), ["val1", "val2", "val3"])
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

assertion failed

### Expected Behaviour

assertion success

### Additional Context

If docs were correct, a `,` (comma) should have been used as a default delimiter. I'd rather see the docs fixed and not introduce an actual default value.

### Debug Output

_No response_

---

_Label `C-bug` added by @systemtrap on 2024-08-05 21:20_

---

_Referenced in [clap-rs/clap#5635](../../clap-rs/clap/pulls/5635.md) on 2024-08-07 20:26_

---

_Comment by @epage on 2024-08-07 20:30_

This was a bad update to the docs from when we shifted from `use_value_delimiter` to `value_delimiter`.

---

_Closed by @epage on 2024-08-07 20:33_

---

_Closed by @epage on 2024-08-07 20:33_

---
