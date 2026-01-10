---
number: 1733
title: "`+nightly` detection in clap"
type: issue
state: closed
author: xd009642
labels: []
assignees: []
created_at: 2020-03-08T17:05:24Z
updated_at: 2020-03-08T18:24:49Z
url: https://github.com/clap-rs/clap/issues/1733
synced_at: 2026-01-10T01:27:04Z
---

# `+nightly` detection in clap

---

_Issue opened by @xd009642 on 2020-03-08 17:05_

So I'm working on a cargo subcommand and I now call cargo itself a couple of times during application via `std::process::Command`, however I need to detect if the user has added `cargo +nightly` before calling my command and was wondering how to do that with clap?

---

_Label `T: RFC / question` added by @xd009642 on 2020-03-08 17:05_

---

_Comment by @CreepySkeleton on 2020-03-08 18:24_

Let me make this clear: you're working on [a cargo plugin](https://github.com/rust-lang/cargo/wiki/Third-party-cargo-subcommands) and, for some reason, you need to detect if your plugin was called as `cargo +nightly plugin`. 

If I'm right, I believe this is impossible because, [apparently](https://github.com/rust-lang/cargo/blob/bb3f2c59274dbf181f2d1a58c54486df4292ec2a/src/bin/cargo/main.rs#L120-L157), cargo doesn't set any env vars or some info that would possibly allow you to sort it out. `clap` can't do anything about it, you should go to [the cargo bugtracker](https://github.com/rust-lang/cargo/issues). 

---

_Closed by @CreepySkeleton on 2020-03-08 18:24_

---

_Referenced in [rust-lang/cargo#7976](../../rust-lang/cargo/issues/7976.md) on 2020-03-08 18:27_

---
