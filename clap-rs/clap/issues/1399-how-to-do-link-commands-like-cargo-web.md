---
number: 1399
title: How to do link-commands like cargo web ...?
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2018-12-22T14:03:18Z
updated_at: 2018-12-30T19:31:22Z
url: https://github.com/clap-rs/clap/issues/1399
synced_at: 2026-01-10T01:26:51Z
---

# How to do link-commands like cargo web ...?

---

_Issue opened by @ghost on 2018-12-22 14:03_

This is kind of a suggestion. There could probably be a sample in how to write a `SubCommand` that's **_not parsed_** and goes directly to an **_executable_**, i.e., `cargo web`. Me myself I don't know how, but [Cargo uses `clap`](https://github.com/rust-lang/cargo/blob/master/src/bin/cargo/cli.rs). I'm still kinda unsure how that's done...

```
ascargo air cert --pw=ý
ascargo-air.exe cert --pw=ý
```

By '_not parsed'_ I mean it redirects to an executable w/o consuming the given args. Can someone please explain how this is done?

---

_Comment by @ghost on 2018-12-26 09:45_

Already [answered](https://users.rust-lang.org/t/how-to-load-binaries-like-cargo-web-exe-in-clap/23468/3).

---

_Closed by @ghost on 2018-12-30 19:31_

---
