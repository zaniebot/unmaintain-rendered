---
number: 2910
title: "Depending on `=clap 3.0.0-beta4` installs `clap_derive 3.0.0-beta5` and breaks builds"
type: issue
state: closed
author: Kobzol
labels:
  - C-bug
assignees: []
created_at: 2021-10-19T12:05:54Z
updated_at: 2021-10-20T16:18:16Z
url: https://github.com/clap-rs/clap/issues/2910
synced_at: 2026-01-10T01:27:28Z
---

# Depending on `=clap 3.0.0-beta4` installs `clap_derive 3.0.0-beta5` and breaks builds

---

_Issue opened by @Kobzol on 2021-10-19 12:05_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

3.0.0-beta5

### Minimal reproducible code

`src/main.rs`
```rust
use clap::Clap;

#[derive(Clap)]
#[clap(version = "1.0")]
struct Opts {
    a: String,
    #[clap(long, default_value = "7760")]
    b: u16,
}

fn main() {
    let opts = Opts::parse().unwrap();
}
```

`Cargo.toml`
```toml
[package]
name = "claptest"
version = "0.1.0"
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
clap = "=3.0.0-beta.4"
```

### Steps to reproduce the bug with the above code

`cargo check`

### Actual Behaviour

The new beta5 renamed the `Clap` procedural macro to `Parser`. When you try to depend on `clap = "=3.0.0-beta4"`, it will use `clap_derive 3.0.0-beta5`, which breaks previously working builds. I noticed that `clap` depends on the exact version of `clap_derive`, so this shouldn't happen. Maybe it's a Cargo bug?

### Expected Behaviour

It should compile as the previous versions did.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `T: bug` added by @Kobzol on 2021-10-19 12:05_

---

_Comment by @pksunkara on 2021-10-19 12:10_

No, it an issue with the prerelease semver. The only solution here is to upgrade.

---

_Closed by @pksunkara on 2021-10-19 12:10_

---

_Referenced in [clap-rs/clap#2917](../../clap-rs/clap/issues/2917.md) on 2021-10-20 14:14_

---

_Comment by @ghost on 2021-10-20 15:21_

@Kobzol use this
```toml
# clap 3.0.0-beta.2
clap = {git="https://github.com/clap-rs/clap", rev="08b2f4d4289eca8a9225bbc56d5a5ad1e99e38e1"}
```
You need to find the commit hash of the version you want and replace `rev`

Upgrading is not the only solution.

---

_Comment by @Kobzol on 2021-10-20 16:17_

Even when I pinned the version to beta4 or beta2, Cargo was still downloading clap_derive beta5 for some reason. It could be fixed by changing Cargo.lock, but we ended up upgrading anyway.

---

_Referenced in [Nukesor/pueue#256](../../Nukesor/pueue/issues/256.md) on 2021-10-28 04:43_

---
