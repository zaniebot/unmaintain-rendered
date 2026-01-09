---
number: 5604
title: Group Relation Bug
type: issue
state: closed
author: gipsyh
labels:
  - C-bug
assignees: []
created_at: 2024-07-28T07:49:59Z
updated_at: 2024-07-29T13:45:14Z
url: https://github.com/clap-rs/clap/issues/5604
synced_at: 2026-01-07T13:12:20-06:00
---

# Group Relation Bug

---

_Issue opened by @gipsyh on 2024-07-28 07:49_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.81.0-nightly (bcf94dec5 2024-06-23)

### Clap Version

clap = { version = "4.5.11", features = ["derive"] }

### Minimal reproducible code

```rust
use clap::{ArgGroup, Parser};

#[derive(Parser)]
#[command(group = ArgGroup::new("engine").required(true).multiple(false))]
pub struct Options {
    #[arg(long, group = "engine")]
    pub a: bool,

    #[arg(long, requires = "a")]
    pub aa: bool,

    #[arg(long, group = "engine")]
    pub b: bool,

    
}

fn main() {
    let args = Options::parse();
}
```


### Steps to reproduce the bug with the above code

cargo r -- --b --aa

### Actual Behaviour

it passed checking.

### Expected Behaviour

option "--aa" depands "--a", it should be error

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @gipsyh on 2024-07-28 07:49_

---

_Comment by @epage on 2024-07-29 13:36_

Reproduced it
```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { version = "4.5.11", features = ["derive"] }
---

use clap::{ArgGroup, Parser};

#[derive(Parser, Debug)]
#[command(group = ArgGroup::new("engine").required(true).multiple(false))]
pub struct Options {
    #[arg(long, group = "engine")]
    pub a: bool,

    #[arg(long, requires = "a")]
    pub aa: bool,

    #[arg(long, group = "engine")]
    pub b: bool,
}

fn main() {
    let args = Options::parse();
    dbg!(&args);
}
```

---

_Comment by @epage on 2024-07-29 13:45_

This appears to be a duplicate of #4520.  While that is using `conflicts_with`, mutually-exclusive groups have the same affect.  Closing in favor of that issue.

if there is a reason to keep this open separately, let us know!

---

_Closed by @epage on 2024-07-29 13:45_

---
