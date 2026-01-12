```yaml
number: 2405
title: Using subcmd field name in subcommand derive
type: issue
state: closed
author: omar25h
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2021-03-10T18:06:18Z
updated_at: 2021-03-10T20:18:55Z
url: https://github.com/clap-rs/clap/issues/2405
synced_at: 2026-01-12T16:14:13Z
```

# Using subcmd field name in subcommand derive

---

_@omar25h_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.50.0 (cb75ad5db 2021-02-10)

### Clap Version

master

### Minimal reproducible code

```rust
use clap::Clap;

fn main() {
    let _opts = Opts::parse();
}

#[derive(Clap)]
struct Opts {
    #[clap(subcommand)]
    subcmd: SubCommand,
}

#[derive(Clap)]
enum SubCommand {
    Test,
}
```


### Steps to reproduce the bug with the above code

cargo build

### Actual Behaviour

```
error[E0308]: mismatched types
  --> src/main.rs:10:13
   |
10 |     subcmd: SubCommand,
   |             ^^^^^^^^^^ expected enum `Option`, found `&mut SubCommand`
   |
   = note:           expected enum `Option<(&str, &ArgMatches)>`
           found mutable reference `&mut SubCommand`

error: aborting due to previous error

For more information about this error, try `rustc --explain E0308`.
```

### Expected Behaviour

The program compiles.

### Additional Context

This issue is initially discovered in #2375. Filing as a separate issue because that's different from the original issue described there.

### Debug Output

_No response_



---

_Label `T: bug` added by @omar25h on 2021-03-10 18:06_

---

_Referenced in [clap-rs/clap#2406](../../clap-rs/clap/pulls/2406.md) on 2021-03-10 18:07_

---

_Added to milestone `3.0` by @pksunkara on 2021-03-10 20:09_

---

_Closed by @pksunkara on 2021-03-10 20:11_

---

_Label `C: derive macros` added by @pksunkara on 2021-03-10 20:18_

---
