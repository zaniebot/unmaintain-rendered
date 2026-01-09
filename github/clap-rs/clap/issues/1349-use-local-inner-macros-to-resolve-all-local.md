---
number: 1349
title: Use local_inner_macros to resolve all local macros within crate
type: issue
state: closed
author: csmoe
labels: []
assignees: []
created_at: 2018-09-28T16:20:13Z
updated_at: 2020-02-02T05:09:07Z
url: https://github.com/clap-rs/clap/issues/1349
synced_at: 2026-01-07T13:12:19-06:00
---

# Use local_inner_macros to resolve all local macros within crate

---

_Issue opened by @csmoe on 2018-09-28 16:20_

https://play.rust-lang.org/?gist=ad478428a2e018819b07ae7e381f662a&version=nightly&mode=debug&edition=2018
cannot import macros from clap in rustc edition 2018: 
```rust
#![deny(rust_2018_idioms)]

// works
// #[macro_use]
// extern crate clap; // 2.32.0

// err
use clap::app_from_crate;

fn main() {
  app_from_crate!();
}
```
cc https://github.com/rust-lang/rust/issues/54642

---

_Comment by @mkpankov on 2019-02-01 12:05_

Is anyone looking at this? It's a significant paper-cut

---

_Comment by @CreepySkeleton on 2020-02-02 05:08_

Closing as duplicate of https://github.com/clap-rs/clap/issues/1478 

---

_Closed by @CreepySkeleton on 2020-02-02 05:08_

---

_Label `T: duplicate` added by @CreepySkeleton on 2020-02-02 05:09_

---
