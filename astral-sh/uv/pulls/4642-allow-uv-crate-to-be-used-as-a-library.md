```yaml
number: 4642
title: "Allow `uv` crate to be used as a library"
type: pull_request
state: merged
author: zanieb
labels:
  - rustlib
assignees: []
merged: true
base: main
head: zb/lib
created_at: 2024-06-29T03:52:30Z
updated_at: 2024-07-11T04:00:19Z
url: https://github.com/astral-sh/uv/pull/4642
synced_at: 2026-01-10T13:42:52Z
```

# Allow `uv` crate to be used as a library

---

_Pull request opened by @zanieb on 2024-06-29 03:52_

This is pulled out of #4632 — a user noted that it would be useful to use the `uv` crate from Rust. This makes it way easier to invoke `uv` from Rust with arbitrary arguments as well as use various functionality in the `uv` crate.

Note this is no longer needed for #4632 and is not particularly urgent.

---

_Label `rustlib` added by @zanieb on 2024-06-29 03:52_

---

_@zanieb reviewed on 2024-06-29 04:31_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:987 on 2024-06-29 04:31_

This moved without change from `run` above because it's not safe to send `std::env` values across threads depending on the platform.

---

_Marked ready for review by @zanieb on 2024-06-29 04:31_

---

_Review requested from @BurntSushi by @zanieb on 2024-07-02 02:47_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-02 02:47_

---

_@BurntSushi reviewed on 2024-07-08 16:56_

I am a little unsure of this personally. Maybe it's fine, but I think it would warrant a docstring somewhere saying that this is not officially supported and to Use At Your Own Risk.

My main specific worry is that this will enable calling the entire `uv` application more than once in the same process. I can't think of anything off the top of my head that will definitely break as a result of this, but if we're leaving state around that gets reused, it could be an issue.

My more vague worry is that we didn't really design `uv` to be invoked as a library. So there may be other things that can go wrong here.

---

_Comment by @sgammon on 2024-07-08 17:18_

Hey `uv` team,

We wanted to share our use case for `uv` as a library; we are building a polyglot runtime, [Elide](https://github.com/elide-dev/elide), which runs Python as part of its supported language engines.

We want to embed a toolchain to obtain dependencies and manage `.venv`. `uv` is perfect for this and architected quite well, so it pops out at us as a good option.

Our dispatch style is a bit weird: we are calling `uv` over a JNI border from the JVM. Other than this, our effective usage of `uv` should end up being pretty close to what the binary experiences standalone: (1) we intend to only run one `uv` execution per process, with `uv` taking over when the user invokes it; (2) we are calling `uv` over the JNI border with an array of string args, alleviating any need for type sharing or serialization.

We are happy to continue using `uv` this way even if there is no official API surface yet. It would be cool eventually to surface structured args, but we're happy with strings as long as we must use them.

It is possible, of course, to ship `uv` as a standalone binary, and invoke it regularly as a subprocess. I considered this route but there are drawbacks:

- We are in Rust too, and could leverage all the optimization and protection of dispatching over Rust
- We end up embedding many of the same crate dependencies anyway, so this would actually increase duplication

All in all if you guys strongly feel like `uv` should be dispatched over a process border instead, we are happy to consider it.

Of course thank you for all your hard work on `uv` and everything else Astral 😄 

---

_@konstin reviewed on 2024-07-08 21:26_

I like this because it's helpful for some testing improvements i want to try. There are some differences between using uv through the main function from rust and calling it as a subprocess (we have some global and thread local state), so i'd keep our officially supported and stable interface to calling binary with cli arguments.

---

_@konstin approved on 2024-07-08 21:26_

---

_Comment by @zanieb on 2024-07-10 15:48_

I added a warning

---

_Review requested from @BurntSushi by @zanieb on 2024-07-10 15:57_

---

_@BurntSushi approved on 2024-07-10 16:53_

---

_Merged by @zanieb on 2024-07-10 17:15_

---

_Closed by @zanieb on 2024-07-10 17:15_

---

_Branch deleted on 2024-07-10 17:15_

---

_Comment by @dsully on 2024-07-11 03:34_

I have a use for this as well. Are you planning on publishing `uv` to crates.io for consumption?

There is an existing 'uv' "universal getter" that has the name currently, but looks abandoned.

---

_Comment by @zanieb on 2024-07-11 03:56_

We aren't prioritizing it — we're far from a stable Rust API and we move very quickly which sometimes requires a `git` patch for a dependency. We are interested in doing so eventually though.

---

_Comment by @zanieb on 2024-07-11 03:56_

What's your use case? :)

---

_Comment by @dsully on 2024-07-11 03:59_

We have an internal tool that sets up Python environments for developers: The venv, tox, dependencies, etc.

I also have need for calling `uv tool install ...` for certain tools that are venv-independent.

Unfortunately I can't use `git+https://...` for dependencies.

---
