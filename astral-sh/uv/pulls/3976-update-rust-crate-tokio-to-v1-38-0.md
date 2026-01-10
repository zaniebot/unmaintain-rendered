```yaml
number: 3976
title: Update Rust crate tokio to v1.38.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/tokio-1.x-lockfile
created_at: 2024-06-03T01:35:38Z
updated_at: 2024-06-03T01:51:40Z
url: https://github.com/astral-sh/uv/pull/3976
synced_at: 2026-01-10T13:59:34Z
```

# Update Rust crate tokio to v1.38.0

---

_Pull request opened by @renovate on 2024-06-03 01:35_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [tokio](https://tokio.rs) ([source](https://togithub.com/tokio-rs/tokio)) | dev-dependencies | minor | `1.37.0` -> `1.38.0` |
| [tokio](https://tokio.rs) ([source](https://togithub.com/tokio-rs/tokio)) | workspace.dependencies | minor | `1.37.0` -> `1.38.0` |

---

### Release Notes

<details>
<summary>tokio-rs/tokio (tokio)</summary>

### [`v1.38.0`](https://togithub.com/tokio-rs/tokio/releases/tag/tokio-1.38.0): Tokio v1.38.0

[Compare Source](https://togithub.com/tokio-rs/tokio/compare/tokio-1.37.0...tokio-1.38.0)

This release marks the beginning of stabilization for runtime metrics. It
stabilizes `RuntimeMetrics::worker_count`. Future releases will continue to
stabilize more metrics.

##### Added

-   fs: add `File::create_new` ([#&#8203;6573])
-   io: add `copy_bidirectional_with_sizes` ([#&#8203;6500])
-   io: implement `AsyncBufRead` for `Join` ([#&#8203;6449])
-   net: add Apple visionOS support ([#&#8203;6465])
-   net: implement `Clone` for `NamedPipeInfo` ([#&#8203;6586])
-   net: support QNX OS ([#&#8203;6421])
-   sync: add `Notify::notify_last` ([#&#8203;6520])
-   sync: add `mpsc::Receiver::{capacity,max_capacity}` ([#&#8203;6511])
-   sync: add `split` method to the semaphore permit ([#&#8203;6472], [#&#8203;6478])
-   task: add `tokio::task::join_set::Builder::spawn_blocking` ([#&#8203;6578])
-   wasm: support rt-multi-thread with wasm32-wasi-preview1-threads ([#&#8203;6510])

##### Changed

-   macros: make `#[tokio::test]` append `#[test]` at the end of the attribute list ([#&#8203;6497])
-   metrics: fix `blocking_threads` count ([#&#8203;6551])
-   metrics: stabilize `RuntimeMetrics::worker_count` ([#&#8203;6556])
-   runtime: move task out of the `lifo_slot` in `block_in_place` ([#&#8203;6596])
-   runtime: panic if `global_queue_interval` is zero ([#&#8203;6445])
-   sync: always drop message in destructor for oneshot receiver ([#&#8203;6558])
-   sync: instrument `Semaphore` for task dumps ([#&#8203;6499])
-   sync: use FIFO ordering when waking batches of wakers ([#&#8203;6521])
-   task: make `LocalKey::get` work with Clone types ([#&#8203;6433])
-   tests: update nix and mio-aio dev-dependencies ([#&#8203;6552])
-   time: clean up implementation ([#&#8203;6517])
-   time: lazily init timers on first poll ([#&#8203;6512])
-   time: remove the `true_when` field in `TimerShared` ([#&#8203;6563])
-   time: use sharding for timer implementation ([#&#8203;6534])

##### Fixed

-   taskdump: allow building taskdump docs on non-unix machines ([#&#8203;6564])
-   time: check for overflow in `Interval::poll_tick` ([#&#8203;6487])
-   sync: fix incorrect `is_empty` on mpsc block boundaries ([#&#8203;6603])

##### Documented

-   fs: rewrite file system docs ([#&#8203;6467])
-   io: fix `stdin` documentation ([#&#8203;6581])
-   io: fix obsolete reference in `ReadHalf::unsplit()` documentation ([#&#8203;6498])
-   macros: render more comprehensible documentation for `select!` ([#&#8203;6468])
-   net: add missing types to module docs ([#&#8203;6482])
-   net: fix misleading `NamedPipeServer` example ([#&#8203;6590])
-   sync: add examples for `SemaphorePermit`, `OwnedSemaphorePermit` ([#&#8203;6477])
-   sync: document that `Barrier::wait` is not cancel safe ([#&#8203;6494])
-   sync: explain relation between `watch::Sender::{subscribe,closed}` ([#&#8203;6490])
-   task: clarify that you can't abort `spawn_blocking` tasks ([#&#8203;6571])
-   task: fix a typo in doc of `LocalSet::run_until` ([#&#8203;6599])
-   time: fix test-util requirement for pause and resume in docs ([#&#8203;6503])

[#&#8203;6421]: https://togithub.com/tokio-rs/tokio/pull/6421

[#&#8203;6433]: https://togithub.com/tokio-rs/tokio/pull/6433

[#&#8203;6445]: https://togithub.com/tokio-rs/tokio/pull/6445

[#&#8203;6449]: https://togithub.com/tokio-rs/tokio/pull/6449

[#&#8203;6465]: https://togithub.com/tokio-rs/tokio/pull/6465

[#&#8203;6467]: https://togithub.com/tokio-rs/tokio/pull/6467

[#&#8203;6468]: https://togithub.com/tokio-rs/tokio/pull/6468

[#&#8203;6472]: https://togithub.com/tokio-rs/tokio/pull/6472

[#&#8203;6477]: https://togithub.com/tokio-rs/tokio/pull/6477

[#&#8203;6478]: https://togithub.com/tokio-rs/tokio/pull/6478

[#&#8203;6482]: https://togithub.com/tokio-rs/tokio/pull/6482

[#&#8203;6487]: https://togithub.com/tokio-rs/tokio/pull/6487

[#&#8203;6490]: https://togithub.com/tokio-rs/tokio/pull/6490

[#&#8203;6494]: https://togithub.com/tokio-rs/tokio/pull/6494

[#&#8203;6497]: https://togithub.com/tokio-rs/tokio/pull/6497

[#&#8203;6498]: https://togithub.com/tokio-rs/tokio/pull/6498

[#&#8203;6499]: https://togithub.com/tokio-rs/tokio/pull/6499

[#&#8203;6500]: https://togithub.com/tokio-rs/tokio/pull/6500

[#&#8203;6503]: https://togithub.com/tokio-rs/tokio/pull/6503

[#&#8203;6510]: https://togithub.com/tokio-rs/tokio/pull/6510

[#&#8203;6511]: https://togithub.com/tokio-rs/tokio/pull/6511

[#&#8203;6512]: https://togithub.com/tokio-rs/tokio/pull/6512

[#&#8203;6517]: https://togithub.com/tokio-rs/tokio/pull/6517

[#&#8203;6520]: https://togithub.com/tokio-rs/tokio/pull/6520

[#&#8203;6521]: https://togithub.com/tokio-rs/tokio/pull/6521

[#&#8203;6534]: https://togithub.com/tokio-rs/tokio/pull/6534

[#&#8203;6551]: https://togithub.com/tokio-rs/tokio/pull/6551

[#&#8203;6552]: https://togithub.com/tokio-rs/tokio/pull/6552

[#&#8203;6556]: https://togithub.com/tokio-rs/tokio/pull/6556

[#&#8203;6558]: https://togithub.com/tokio-rs/tokio/pull/6558

[#&#8203;6563]: https://togithub.com/tokio-rs/tokio/pull/6563

[#&#8203;6564]: https://togithub.com/tokio-rs/tokio/pull/6564

[#&#8203;6571]: https://togithub.com/tokio-rs/tokio/pull/6571

[#&#8203;6573]: https://togithub.com/tokio-rs/tokio/pull/6573

[#&#8203;6578]: https://togithub.com/tokio-rs/tokio/pull/6578

[#&#8203;6581]: https://togithub.com/tokio-rs/tokio/pull/6581

[#&#8203;6586]: https://togithub.com/tokio-rs/tokio/pull/6586

[#&#8203;6590]: https://togithub.com/tokio-rs/tokio/pull/6590

[#&#8203;6596]: https://togithub.com/tokio-rs/tokio/pull/6596

[#&#8203;6599]: https://togithub.com/tokio-rs/tokio/pull/6599

[#&#8203;6603]: https://togithub.com/tokio-rs/tokio/pull/6603

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about these updates again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4zNzcuOCIsInVwZGF0ZWRJblZlciI6IjM3LjM3Ny44IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-06-03 01:35_

---

_Merged by @charliermarsh on 2024-06-03 01:51_

---

_Closed by @charliermarsh on 2024-06-03 01:51_

---

_Branch deleted on 2024-06-03 01:51_

---
