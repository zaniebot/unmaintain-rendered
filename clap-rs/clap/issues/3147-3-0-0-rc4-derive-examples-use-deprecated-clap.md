---
number: 3147
title: "3.0.0-rc4: derive examples use deprecated clap::Subcommand"
type: issue
state: closed
author: japert
labels:
  - C-bug
assignees: []
created_at: 2021-12-10T22:55:19Z
updated_at: 2021-12-11T00:03:03Z
url: https://github.com/clap-rs/clap/issues/3147
synced_at: 2026-01-10T01:27:34Z
---

# 3.0.0-rc4: derive examples use deprecated clap::Subcommand

---

_Issue opened by @japert on 2021-12-10 22:55_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

3.0.0-rc.4

### Minimal reproducible code

```rust
use clap::SubCommand;
fn main() {}
```


### Steps to reproduce the bug with the above code

cargo build

### Actual Behaviour

warning: use of deprecated struct `clap::SubCommand`: Replaced with `App`
 --> src/main.rs:1:28
  |
1 | use clap::SubCommand;

### Expected Behaviour

The examples should not use deprecated code.

### Additional Context

For example examples/git_derive.rs, but also other subcommand examples.


### Debug Output

_No response_

---

_Label `C-bug` added by @japert on 2021-12-10 22:55_

---

_Added to milestone `3.0` by @pksunkara on 2021-12-10 23:26_

---

_Comment by @japert on 2021-12-11 00:00_

Oh. I just noticed that there is both `clap::Subcommand` and `clap::SubCommand`.
`clap::SubCommand` is deprecated, `clap::Subcommand` is not.
The examples use `clap::Subcommand`, so I think this issue is moot and can be closed.

My autocomplete chose the wrong thing and I had a case of case blindness. I guess it's a good thing one of the two disappears soon haha.

---

_Closed by @japert on 2021-12-11 00:00_

---

_Referenced in [clap-rs/clap#3150](../../clap-rs/clap/pulls/3150.md) on 2021-12-11 00:18_

---
