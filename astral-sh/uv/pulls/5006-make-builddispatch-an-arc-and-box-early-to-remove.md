```yaml
number: 5006
title: "Make `BuildDispatch` an `Arc` and box early to remove `UV_STACK_SIZE` hack"
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
draft: true
base: main
head: konsti/mark-everything-send-sync
created_at: 2024-07-12T12:14:11Z
updated_at: 2024-09-24T10:33:22Z
url: https://github.com/astral-sh/uv/pull/5006
synced_at: 2026-01-10T12:53:31Z
```

# Make `BuildDispatch` an `Arc` and box early to remove `UV_STACK_SIZE` hack

---

_Pull request opened by @konstin on 2024-07-12 12:14_

When looking into the stack size problems with debug mode windows, i saw two things:
* Our stack grows deep (>200 frames) when recursively resolving and installing requirements.
* Our early methods (`main`, `run`, etc.) use exceedingly much stack.

For the first problem, i made `BuildDispatch` an `Arc` and spawned the recursive resolution and installation with `tokio::task::spawn`. It has the nice side effect that most async functions become `Send` and it allows us to spawn arbitrary other futures if we want.

For the second problem, i aggressively boxed futures, in the current diff somewhat arbitrarily.

Together, the test suite passes with 1MB of stack on my linux machine. It should allows us remove the `UV_STACK_SIZE` hack on windows.

---

_Review comment by @BurntSushi on `crates/bench/benches/uv.rs`:162 on 2024-07-12 12:18_

[auto-claim when?](https://smallcultfollowing.com/babysteps/blog/2024/06/21/claim-auto-and-otherwise/)

---

_@BurntSushi reviewed on 2024-07-12 12:21_

Nice! I think the main thing that bothers me here is boxing futures everywhere. Not so much because I'm worried about it impacting perf necessarily, but like, for someone else editing code, how do they know when a future should be boxed? Do we have any feedback mechanism other than "stack exceeded" in CI on Windows?

---

_Comment by @konstin on 2024-07-12 12:27_

Yeah i forgot that in the PR description: If we commit to changing this i will prune down the boxing. Unfortunately i don't think we have a good measuring system. The best approach would be to have less locals in the high level functions, instead group them and then box the group, but otherwise it comes down to checking the type size and boxing the largest future.

---

_Comment by @BurntSushi on 2024-07-12 13:11_

Yeah that makes sense. What I figured.

@ibraheemdev Do you have thoughts on this change?

---

_Comment by @ibraheemdev on 2024-07-12 15:28_

We actually don't need to introduce the `Send` requirement here if we use [`tokio::spawn_local`](https://docs.rs/tokio/latest/tokio/task/fn.spawn_local.html) and wrap `run` in a [`LocalSet`](https://docs.rs/tokio/latest/tokio/task/struct.LocalSet.html#method.run_until).

---

_Closed by @konstin on 2024-09-24 10:33_

---
