```yaml
number: 2108
title: "#![deny(missing_docs)] incompatible with #[clap(flatten)]"
type: issue
state: closed
author: jared-mackey
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2020-08-25T04:00:33Z
updated_at: 2020-08-26T07:36:59Z
url: https://github.com/clap-rs/clap/issues/2108
synced_at: 2026-01-12T16:14:12Z
```

# #![deny(missing_docs)] incompatible with #[clap(flatten)]

---

_@jared-mackey_

### Code

```rust
use clap::Clap;

/// Docs included
#[derive(Clap)]
#[clap(author, version)]
struct Opts {
    /// Document debug
    #[clap(short, long)]
    debug: bool,

    /// Flatten
    #[clap(flatten)]
    thing: ThingConfig,

}

/// Thing CLI Configuration
#[derive(Clap)]
#[clap(name = "thing")]
struct ThingConfig {
    /// Thing URI
    #[clap(long)]
    uri: String,
}
```

### Steps to reproduce the issue

1. Run `cargo build`
2. ???
3. PROFIT!!!

### Version

* **Rust**: rustc 1.47.0-nightly (de521cbb3 2020-08-21)

* **Clap**: "3.0.0-beta.1"

### Actual Behavior Summary

When compiling with a doc on the flattened `thing: ThingConfig`

```rust
error: methods and doc comments are not allowed for flattened entry
  --> src/clapped.rs
   |
45 |     #[clap(flatten)]
   |            ^^^^^^^

```

When compiling without it (since I can't)

```rust
error: missing documentation for the crate
```

### Expected Behavior Summary

Flattened options should not raise a missing docs warning.


---

_Label `T: bug` added by @jared-mackey on 2020-08-25 04:00_

---

_Comment by @CreepySkeleton on 2020-08-25 07:19_

I have recently fixed this in structopt but forgot to port the fix here. Will do ASAP

---

_Assigned to @CreepySkeleton by @CreepySkeleton on 2020-08-25 07:19_

---

_Referenced in [clap-rs/clap#2110](../../clap-rs/clap/pulls/2110.md) on 2020-08-25 08:06_

---

_Referenced in [clap-rs/clap#2111](../../clap-rs/clap/pulls/2111.md) on 2020-08-25 08:06_

---

_Label `C: derive macros` added by @pksunkara on 2020-08-25 08:11_

---

_Added to milestone `3.0` by @pksunkara on 2020-08-25 08:11_

---

_Closed by @bors[bot] on 2020-08-26 07:36_

---
