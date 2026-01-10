```yaml
number: 18033
title: disable jemalloc on android
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
assignees: []
merged: true
base: main
head: micha/disable-jemalloc-android
created_at: 2025-05-12T07:19:56Z
updated_at: 2025-05-12T12:52:29Z
url: https://github.com/astral-sh/ruff/pull/18033
synced_at: 2026-01-10T18:51:01Z
```

# disable jemalloc on android

---

_Pull request opened by @MichaReiser on 2025-05-12 07:19_

## Summary

The android ndk no longer ships with gcc by default, but jemalloc requires it. 
There are some [tricks](https://github.com/astral-sh/ruff/issues/17527#issuecomment-2824783704) that allow building jemalloc without gcc but this feels unnecessarily complicated.

For now, let's disable jemalloc.

Fixes https://github.com/astral-sh/ruff/issues/18016
Related https://github.com/astral-sh/ruff/issues/17527

## Test Plan

I don't have an android device.


---

_Review requested from @carljm by @MichaReiser on 2025-05-12 07:19_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-12 07:19_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-12 07:19_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-12 07:19_

---

_Label `cli` added by @MichaReiser on 2025-05-12 07:20_

---

_Comment by @MichaReiser on 2025-05-12 07:20_

@ZeusKC52 could you try if this version works fine for you on android?

---

_Review request for @dcreager removed by @MichaReiser on 2025-05-12 07:21_

---

_Review request for @carljm removed by @MichaReiser on 2025-05-12 07:21_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-05-12 07:21_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-05-12 07:21_

---

_Review requested from @ntBre by @MichaReiser on 2025-05-12 07:21_

---

_Comment by @github-actions[bot] on 2025-05-12 07:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @ZeusKC52 on 2025-05-12 07:25_

How do i run this on termux?

---

_Comment by @github-actions[bot] on 2025-05-12 07:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-05-12 07:30_

> How do i run this on termux?

Not sure, I never used termux. 

YOu can clone this repository and then run `cargo build --bin ruff`. But I don't know how involved that is. 

The alternative is we land this PR and you wait for the next release to test it out and you report back in case it doesn't work.

---

_Comment by @ZeusKC52 on 2025-05-12 11:41_

How do i clone this pull request? I looked on the internet but nothing seems to work.

---

_Review comment by @ntBre on `crates/ty/src/main.rs`:11 on 2025-05-12 12:33_

This looks like it might be unrelated

---

_@ntBre approved on 2025-05-12 12:33_

Thanks, this makes sense to me!

---

_Comment by @ntBre on 2025-05-12 12:37_

> How do i clone this pull request? I looked on the internet but nothing seems to work.

You can run `git clone` with the `-b`  flag (short for `--branch`):

```shell
git clone -b micha/disable-jemalloc-android https://github.com/astral-sh/ruff --depth 1
```

I also threw in `--depth 1` to avoid pulling down the whole history if you just want to test this once.

I have a couple of old android devices I can possibly try to charge if needed too.

---

_Comment by @MichaReiser on 2025-05-12 12:40_

Let's test this with the next release. 

---

_Merged by @MichaReiser on 2025-05-12 12:41_

---

_Closed by @MichaReiser on 2025-05-12 12:41_

---

_Branch deleted on 2025-05-12 12:41_

---

_Comment by @ZeusKC52 on 2025-05-12 12:46_

Tried and it gave me this:

$ git clone -b micha/disable-jemalloc-android https://github.com/astral-sh/ruff --depth 1
fatal: destination path 'ruff' already exists and is not an empty directory.

What now?

---

_Comment by @ntBre on 2025-05-12 12:52_

The branch has now been merged into the `main` branch, so you can just go into the ruff directory and run the command Micha mentioned above, after pulling the new changes:

```shell
cd ruff
git pull
cargo build --bin ruff
```

Or you can wait for the next release later this week!

---
