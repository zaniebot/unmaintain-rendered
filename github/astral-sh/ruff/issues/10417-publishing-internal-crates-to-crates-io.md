---
number: 10417
title: Publishing Internal Crates to crates.io
type: issue
state: closed
author: mfish332
labels:
  - release
assignees: []
created_at: 2024-03-15T02:19:22Z
updated_at: 2025-04-15T04:37:19Z
url: https://github.com/astral-sh/ruff/issues/10417
synced_at: 2026-01-07T13:12:15-06:00
---

# Publishing Internal Crates to crates.io

---

_Issue opened by @mfish332 on 2024-03-15 02:19_

Hi thanks for this awesome project. I am working on a project that does dependency analysis on a very large codebase and would love to use Rust. When looking for the best Rust python parser I was led to [RustPython/Parser](https://github.com/RustPython/Parser). I see that Ruff originally used RustPython/Parser but it seems like the current parser has deviated quite a bit. I am wondering if you can publicly publish some of the internal crates such as the parser and ast packages. 

---

_Comment by @zanieb on 2024-03-15 03:04_

This is loosely a duplicate of https://github.com/astral-sh/ruff/issues/43

We're hesitant to publish to crates.io at this time because the internal crates are not versioned and we are moving too fast to commit to a stable Rust API.

---

_Comment by @mfish332 on 2024-03-15 03:09_

A possible solution would be to publish at a `0.0.x` version and not commit to any API. Then people consuming the package could specify an exact version. It is significantly better than forking and having to sync upstream. Ruff really has the best rust python parser around and it would be great to be able to use it in other projects.

---

_Comment by @zanieb on 2024-03-15 03:15_

I'm not sure if we can deal with the overhead of versioning and releasing everything separately, maybe we could just auto-increment the patch on each Ruff release.

I'll chat with the team though and report back if it feels like we can do it.

Glad you're finding the project useful :) if you're interested in a parser keep an eye on #10036, we're going to replace our existing parser entirely.

---

_Label `release` added by @charliermarsh on 2024-03-15 13:53_

---

_Comment by @MichaReiser on 2024-03-18 13:43_

I'm open to releasing some crates (like AST and parser) without any stability guarantees to crates.io (e.g. using 0.0.x) but we probably would need help to set up the release process. But I'm deferring the decision to @zanieb who's taken the lead here.

---

_Comment by @BurntSushi on 2024-03-18 13:49_

@mfish332 Can you say more about why you need Ruff's crates on crates.io? Can you use git dependencies instead? It does mean that _your_ crate won't be able to be on crates.io which is kind of a bummer. But otherwise there isn't much of a difference between a `0.0.x` release policy and git dependencies (neither really benefit from semver since every change is potentially breaking).

The main concern I have here is indeed needing to worry about semver. An internal crate that isn't published is really a different beast entirely from a crate that is published that others are consuming. The former means we can iterate without any care with respect to breaking changes.

I do suppose that a `0.0.x` version policy would assuage those concerns because every release will be treated as semver incompatible. And if it can be fully automated, then I think that would eliminate most or all of the overhead, aside from maintaining the automation. I don't know much work that would be.

---

_Comment by @mfish332 on 2024-03-22 21:20_

Hi All, thank you for quick the responses. I was mistakenly under the impression that it was impossible to install crates from within a cargo workspace via git (due to path based links). This is not true though, See [SO question](https://stackoverflow.com/questions/73233872/how-do-i-depend-on-a-crate-within-a-cargo-workspace-over-git) or this [cargo issue](https://github.com/rust-lang/cargo/issues/10151)

 For my purposes this will work great as I am not publishing to crates.io, and I can pin the ruff deps to a release tag. TLDR for those coming across this issue at a later point you can run:
```bash
cargo add ruff_python_parser ruff_python_ast --git https://github.com/astral-sh/ruff --tag v0.3.2
``` 

---

_Closed by @mfish332 on 2024-03-22 21:20_

---

_Comment by @flying-sheep on 2024-04-06 14:23_

After replacing the parser, maybe this could be revisited? It would be great to have a robust Python parser on crates.io

Another advantage would be having documentation on docs.rs

---

_Referenced in [conda-forge/deptry-feedstock#5](../../conda-forge/deptry-feedstock/issues/5.md) on 2024-06-14 09:31_

---

_Referenced in [astral-sh/ruff#14051](../../astral-sh/ruff/issues/14051.md) on 2024-11-01 18:31_

---

_Comment by @chrisjsewell on 2025-04-15 04:37_

I would note rustpython now uses the ruff parser: https://github.com/RustPython/RustPython/pull/5494
It currently has to source from GitHub, which seems really not great at all 😅

---
