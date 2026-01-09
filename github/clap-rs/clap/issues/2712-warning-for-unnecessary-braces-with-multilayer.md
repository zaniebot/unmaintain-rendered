---
number: 2712
title: Warning for unnecessary braces with multilayer subcommand
type: issue
state: closed
author: ModProg
labels:
  - C-bug
assignees: []
created_at: 2021-08-18T00:24:52Z
updated_at: 2021-08-18T08:50:13Z
url: https://github.com/clap-rs/clap/issues/2712
synced_at: 2026-01-07T13:12:19-06:00
---

# Warning for unnecessary braces with multilayer subcommand

---

_Issue opened by @ModProg on 2021-08-18 00:24_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.56.0-nightly (2d2bc94c8 2021-08-15)

### Clap Version

master

### Minimal reproducible code

```rust
fn main() {}

#[derive(clap::Clap)]
enum Command {
    Command {
        #[clap(subcommand)]
        cmd: Option<SubCommand>,
    },
}

#[derive(clap::Clap)]
enum SubCommand {}
```


### Steps to reproduce the bug with the above code

`cargo build`

### Actual Behaviour

```
warning: unnecessary braces around block return value
 --> src/main.rs:6:16
  |
6 |         #[clap(subcommand)]
  |                ^^^^^^^^^^ help: remove these braces
  |
  = note: `#[warn(unused_braces)]` on by default
```

### Expected Behaviour

No warning should be triggered

### Additional Context
This is a regression, it worked in the `.2` beta

---

_Label `T: bug` added by @ModProg on 2021-08-18 00:24_

---

_Referenced in [clap-rs/clap#2713](../../clap-rs/clap/pulls/2713.md) on 2021-08-18 01:55_

---

_Closed by @pksunkara on 2021-08-18 08:50_

---
