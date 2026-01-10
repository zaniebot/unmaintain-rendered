---
number: 3017
title: Nested subcommands in enums gives lots of clippy warnings
type: issue
state: closed
author: sondr3
labels:
  - C-bug
assignees: []
created_at: 2021-11-12T18:29:54Z
updated_at: 2021-11-13T12:01:32Z
url: https://github.com/clap-rs/clap/issues/3017
synced_at: 2026-01-10T01:27:30Z
---

# Nested subcommands in enums gives lots of clippy warnings

---

_Issue opened by @sondr3 on 2021-11-12 18:29_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.56.1 (59eed8a2a 2021-11-01)

### Clap Version

clap = "3.0.0-beta.5"

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Parser, Debug)]
struct Opt {
    #[clap(subcommand)]
    cmd: Option<Cmds>,
}

#[derive(Subcommand, Debug)]
enum Cmds {
    #[clap(subcommand)]
    One(OneCmd),
}

#[derive(Subcommand, Debug)]
enum OneCmd {
    Add { name: String, items: Vec<String> },
}

fn main() {
    Opt::parse();
}
```


### Steps to reproduce the bug with the above code

`cargo clippy`

### Actual Behaviour

```text
❯ cargo clippy
    Checking app v.0.1.0 (/tmp/wat)
warning: this looks like an `else if` but the `else` is missing
  --> src/main.rs:17:15
   |
17 |     Add { name: String, items: Vec<String> },
   |               ^^^^^^^^^^
   |
   = note: `#[warn(clippy::suspicious_else_formatting)]` on by default
   = note: to remove this lint, add the missing `else` or add a new line before the second `if`
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#suspicious_else_formatting

warning: this looks like an `else if` but the `else` is missing
  --> src/main.rs:17:11
   |
17 |     Add { name: String, items: Vec<String> },
   |           ^^^^^^^^^^^^^^^^^^^
   |
   = note: to remove this lint, add the missing `else` or add a new line before the second `if`
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#suspicious_else_formatting

warning: this looks like an `else if` but the `else` is missing
  --> src/main.rs:17:15
   |
17 |     Add { name: String, items: Vec<String> },
   |               ^^^^^^^^^^
   |
   = note: to remove this lint, add the missing `else` or add a new line before the second `if`
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#suspicious_else_formatting
```

### Expected Behaviour

No warnings are displayed.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `T: bug` added by @sondr3 on 2021-11-12 18:29_

---

_Referenced in [clap-rs/clap#3018](../../clap-rs/clap/pulls/3018.md) on 2021-11-12 18:54_

---

_Closed by @bors[bot] on 2021-11-12 22:37_

---

_Comment by @epage on 2021-11-12 22:47_

Thanks for reporting this! Its now resolved in master.  I wish there was a better way of handling warnings in macros, it feels very reactionary.  We don't want to blanket disable all because there are some that might highlight a legitimate issue in our code or user code.

I wonder if macro authors have collaborated at all in sharing the most annoying warnings for code gen...

---

_Comment by @sondr3 on 2021-11-13 12:01_

Thanks for the quick fix, looking forward to a proper Clap v3 release :smile: 

---
