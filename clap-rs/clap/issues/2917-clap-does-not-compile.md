```yaml
number: 2917
title: Clap does not compile
type: issue
state: closed
author: ghost
labels:
  - C-bug
assignees: []
created_at: 2021-10-20T14:14:23Z
updated_at: 2021-10-20T14:56:00Z
url: https://github.com/clap-rs/clap/issues/2917
synced_at: 2026-01-12T16:14:14Z
```

# Clap does not compile

---

_@ghost_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.55

### Clap Version

3.0-beta.5

### Minimal reproducible code

```rust
use clap::*;

/// NEAR Indexer Example
/// Watches for stream of blocks from the chain
#[derive(Debug,Clap)]
#[clap(version = "0.1", author = "Near Inc. <hello@nearprotocol.com>")]
pub(crate) struct Opts {
    /// Sets a custom config dir. Defaults to ~/.near/
    #[clap(short, long)]
    pub home_dir: Option<std::path::PathBuf>,
}

fn main() {
    println!("Hello, world!");
}

```


### Steps to reproduce the bug with the above code

`cargo run --release`

### Actual Behaviour

```
error: cannot find derive macro `Clap` in this scope
 --> src/main.rs:5:16
  |
5 | #[derive(Debug,Clap)]
  |                ^^^^

error: cannot find attribute `clap` in this scope
 --> src/main.rs:6:3
  |
6 | #[clap(version = "0.1", author = "Near Inc. <hello@nearprotocol.com>")]
  |   ^^^^

error: cannot find attribute `clap` in this scope
 --> src/main.rs:9:7
  |
9 |     #[clap(short, long)]
  |       ^^^^

warning: unused import: `clap::*`
 --> src/main.rs:1:5
  |
1 | use clap::*;
  |     ^^^^^^^
  |
  = note: `#[warn(unused_imports)]` on by default

error: aborting due to 3 previous errors; 1 warning emitted

```

### Expected Behaviour

Hello world

### Additional Context

The answer here doesn't make sense https://github.com/clap-rs/clap/issues/2910

3.0.0-beta.2 does not compile now
3.0.0-beta.4 does not compile now
3.0.0-beta.5 does not compile now

3.0.0-beta.2 compiled last month

### Debug Output

_No response_

---

_Label `T: bug` added by @ghost on 2021-10-20 14:14_

---

_Comment by @pksunkara on 2021-10-20 14:51_

You would need to upgrade to beta.5 and change the code accordingly since we have breaking changes.

---

_Closed by @pksunkara on 2021-10-20 14:51_

---

_Comment by @ghost on 2021-10-20 14:52_

Upgrade to what? I just showed you that the latest version does not compile

---

_Comment by @ghost on 2021-10-20 14:54_

Nevermind `change the code accordingly` means I have to go and find what that means.

It'd be great if you can point to the place where I can see exactly what is breaking instead of looking through source code.

---

_Comment by @pksunkara on 2021-10-20 14:56_

https://github.com/clap-rs/clap/blob/master/CHANGELOG.md

---

_Referenced in [chmln/handlr#53](../../chmln/handlr/issues/53.md) on 2021-11-06 19:56_

---

_Referenced in [denisidoro/navi#650](../../denisidoro/navi/issues/650.md) on 2021-12-18 07:50_

---
