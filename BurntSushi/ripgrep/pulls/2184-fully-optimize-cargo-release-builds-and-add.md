```yaml
number: 2184
title: Fully optimize cargo release builds and add debuggable release profile
type: pull_request
state: closed
author: SUPERCILEX
labels: []
assignees: []
base: master
head: patch-1
created_at: 2022-04-17T17:34:00Z
updated_at: 2023-02-02T01:20:46Z
url: https://github.com/BurntSushi/ripgrep/pull/2184
synced_at: 2026-01-12T18:23:14Z
```

# Fully optimize cargo release builds and add debuggable release profile

---

_@SUPERCILEX_

See https://github.com/BurntSushi/ripgrep/issues/595#issuecomment-1100180996

---

_Comment by @SUPERCILEX on 2022-04-17 17:48_

Ah, looks like the tests are using rust 1.52 but this needs 1.57: https://github.com/rust-lang/cargo/pull/9943

---

_Comment by @SUPERCILEX on 2022-06-20 19:27_

@BurntSushi is there any interest in this? I'm wondering if it's worth bumping the MSRV.

---

_Comment by @BurntSushi on 2022-06-20 19:31_

I'm totally fine to bump the MSRV, up to and including the latest Rust stable release.

In general, I don't really look at PRs like this unfortunately until I actually do a ripgrep release. At which point, I'll decide whether to bring it in by actually trying it out and playing around with it. If changes need to be made, I'll just do them by squashing them into your commit. If I didn't do that, then I'd be context switching all of the time and would get nothing done. Since I became a Dad, my release cycles have become longer.

So this PR might sit for some time. But I might be interested. Hard to say without actually giving it a whirl and seeing what it's actually like.

---

_Comment by @SUPERCILEX on 2022-06-20 19:59_

Ok, sounds good! I bumped the MSRV so this should be ready to merge if you decide you like it.

BTW, dunno if people have mentioned this before, but a big reason for supporting cargo installation is that you can use `rustflags = ["-C", "target-cpu=native"]` which will presumably get you slightly higher performance.

---

_Comment by @BurntSushi on 2022-06-20 23:47_

> BTW, dunno if people have mentioned this before, but a big reason for supporting cargo installation is that you can use `rustflags = ["-C", "target-cpu=native"]` which will presumably get you slightly higher performance.

Yeah I'm aware of that. I personally think `cargo install` is just more about convenience. Setting `target-cpu=native` is likely to help a little bit in certain workloads, but ripgrep already uses SIMD in its hot paths even when `target-cpu` is not set at all. I don't even bother compiling with `target-cpu=native` when benchmarking ripgrep. (Partially because I've never really seen it make much of a difference and partially because it's an uncommon build configuration.)

---

_Closed by @SUPERCILEX on 2023-02-02 01:20_

---
