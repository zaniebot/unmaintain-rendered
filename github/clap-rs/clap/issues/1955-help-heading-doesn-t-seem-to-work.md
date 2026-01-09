---
number: 1955
title: "help_heading doesn't seem to work"
type: issue
state: closed
author: CreepySkeleton
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2020-05-31T15:43:08Z
updated_at: 2020-06-01T10:14:16Z
url: https://github.com/clap-rs/clap/issues/1955
synced_at: 2026-01-07T13:12:19-06:00
---

# help_heading doesn't seem to work

---

_Issue opened by @CreepySkeleton on 2020-05-31 15:43_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closes issues

Initially reported as https://github.com/clap-rs/clap/discussions/1954

### Code

```rust
use clap::{App, Arg};

fn main() {
    App::new("app")
        .arg(Arg::from("--flag").help_heading(Some("CUSTOM")))
        .print_help()
        .unwrap();
}
```

### Steps to reproduce the issue

1. Run `cargo run`

### Version

* **Rust**: 1.43.1
* **Clap**: 3.0.0-beta.1

### Actual Behavior Summary
```
app 

USAGE:
    app [FLAGS]

FLAGS:
        --flag       
    -h, --help       Prints help information
    -V, --version    Prints version information
```

### Expected Behavior Summary

```
app 

USAGE:
    app [FLAGS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

CUSTOM:
    --flag
```

### Additional context

`help_heading` doesn't seem to work at all. I checked flags, options and positional args, incuding multiple options and combinations.


---

_Label `T: bug` added by @CreepySkeleton on 2020-05-31 15:43_

---

_Label `C: help message` added by @CreepySkeleton on 2020-05-31 15:43_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-05-31 15:43_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-05-31 15:44_

---

_Referenced in [clap-rs/clap#1958](../../clap-rs/clap/pulls/1958.md) on 2020-06-01 04:43_

---

_Closed by @bors[bot] on 2020-06-01 10:14_

---
