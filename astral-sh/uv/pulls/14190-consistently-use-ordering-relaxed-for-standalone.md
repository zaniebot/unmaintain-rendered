```yaml
number: 14190
title: "Consistently use `Ordering::Relaxed` for standalone atomic use cases"
type: pull_request
state: merged
author: christeefy
labels:
  - enhancement
assignees: []
merged: true
base: main
head: consistent-mem-ordering
created_at: 2025-06-21T15:14:30Z
updated_at: 2025-06-24T19:30:53Z
url: https://github.com/astral-sh/uv/pull/14190
synced_at: 2026-01-10T11:10:43Z
```

# Consistently use `Ordering::Relaxed` for standalone atomic use cases

---

_Pull request opened by @christeefy on 2025-06-21 15:14_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This is a follow-up to https://github.com/astral-sh/uv/pull/13033#discussion_r2137343801. 

The goals of this PR include:
1. applying a consistent memory ordering for (simple) use cases that doesn't involve synchronization/interaction with other data structures (i.e. atomics in `uv-warning` and the one introduced in #13033)
2. relaxing the memory ordering from the strictest [`Ordering::SeqCst`](https://doc.rust-lang.org/nomicon/atomics.html#sequentially-consistent) to [`Ordering::Relaxed`](https://doc.rust-lang.org/nomicon/atomics.html#relaxed) for such use cases.

## Test Plan

<!-- How was it tested? -->
Unsure of how to test here. 

## Dependencies
- [ ] #13033 merged

---

_Renamed from "Use Relaxed memory ordering" to "Consistently use `Ordering::Relaxed` for standalone atomic use cases" by @christeefy on 2025-06-21 15:16_

---

_Converted to draft by @christeefy on 2025-06-21 15:44_

---

_Assigned to @oconnor663 by @konstin on 2025-06-22 13:55_

---

_Review requested from @oconnor663 by @konstin on 2025-06-22 13:55_

---

_Marked ready for review by @christeefy on 2025-06-22 19:30_

---

_@oconnor663 requested changes on 2025-06-23 16:48_

Nice, I've thought about doing the same the last time I looked at this code. (Which was also the first time :smile:) What do you think about making the same change to `uv_configuration::RAYON_PARALLELISM` in this PR? (That is, `crates/uv/src/lib.rs` and `crates/uv-configuration/src/threading.rs`.)

---

_Label `enhancement` added by @konstin on 2025-06-23 16:55_

---

_Comment by @christeefy on 2025-06-23 20:41_

I think `RAYON_PARALLELISM` can be set to `Ordering::Relaxed` as well. 

Here's my thinking (feel free to correct me):
1. `uv_configuration::RAYON_PARALLELISM.store` in `lib.rs` is an independent storing of user configuration
2. It is stored at the start of program execution, much earlier than when the rayon thread pool is created.

---

_Review requested from @oconnor663 by @christeefy on 2025-06-23 20:42_

---

_@oconnor663 approved on 2025-06-23 20:45_

CI flake is a known issue, unrelated.

---

_Merged by @oconnor663 on 2025-06-24 19:30_

---

_Closed by @oconnor663 on 2025-06-24 19:30_

---

_Branch deleted on 2025-06-24 19:30_

---
