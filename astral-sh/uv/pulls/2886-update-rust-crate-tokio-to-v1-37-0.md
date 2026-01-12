```yaml
number: 2886
title: Update Rust crate tokio to v1.37.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/tokio-1.x-lockfile
created_at: 2024-04-08T03:05:33Z
updated_at: 2024-04-08T13:35:01Z
url: https://github.com/astral-sh/uv/pull/2886
synced_at: 2026-01-12T16:05:17Z
```

# Update Rust crate tokio to v1.37.0

---

_@renovate_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [tokio](https://tokio.rs) ([source](https://togithub.com/tokio-rs/tokio)) | dev-dependencies | minor | `1.36.0` -> `1.37.0` |
| [tokio](https://tokio.rs) ([source](https://togithub.com/tokio-rs/tokio)) | workspace.dependencies | minor | `1.36.0` -> `1.37.0` |

---

### Release Notes

<details>
<summary>tokio-rs/tokio (tokio)</summary>

### [`v1.37.0`](https://togithub.com/tokio-rs/tokio/releases/tag/tokio-1.37.0): Tokio v1.37.0

[Compare Source](https://togithub.com/tokio-rs/tokio/compare/tokio-1.36.0...tokio-1.37.0)

### 1.37.0 (March 28th, 2024)

##### Added

-   fs: add `set_max_buf_size` to `tokio::fs::File` ([#&#8203;6411])
-   io: add `try_new` and `try_with_interest` to `AsyncFd` ([#&#8203;6345])
-   sync: add `forget_permits` method to semaphore ([#&#8203;6331])
-   sync: add `is_closed`, `is_empty`, and `len` to mpsc receivers ([#&#8203;6348])
-   sync: add a `rwlock()` method to owned `RwLock` guards ([#&#8203;6418])
-   sync: expose strong and weak counts of mpsc sender handles ([#&#8203;6405])
-   sync: implement `Clone` for `watch::Sender` ([#&#8203;6388])
-   task: add `TaskLocalFuture::take_value` ([#&#8203;6340])
-   task: implement `FromIterator` for `JoinSet` ([#&#8203;6300])

##### Changed

-   io: make `io::split` use a mutex instead of a spinlock ([#&#8203;6403])

##### Fixed

-   docs: fix docsrs build without net feature ([#&#8203;6360])
-   macros: allow select with only else branch ([#&#8203;6339])
-   runtime: fix leaking registration entries when os registration fails ([#&#8203;6329])

##### Documented

-   io: document cancel safety of `AsyncBufReadExt::fill_buf` ([#&#8203;6431])
-   io: document cancel safety of `AsyncReadExt`'s primitive read functions ([#&#8203;6337])
-   runtime: add doc link from `Runtime` to `#[tokio::main]` ([#&#8203;6366])
-   runtime: make the `enter` example deterministic ([#&#8203;6351])
-   sync: add Semaphore example for limiting the number of outgoing requests ([#&#8203;6419])
-   sync: fix missing period in broadcast docs ([#&#8203;6377])
-   sync: mark `mpsc::Sender::downgrade` with `#[must_use]` ([#&#8203;6326])
-   sync: reorder `const_new` before `new_with` ([#&#8203;6392])
-   sync: update watch channel docs ([#&#8203;6395])
-   task: fix documentation links ([#&#8203;6336])

##### Changed (unstable)

-   runtime: include task `Id` in taskdumps ([#&#8203;6328])
-   runtime: panic if `unhandled_panic` is enabled when not supported ([#&#8203;6410])

[#&#8203;6300]: https://togithub.com/tokio-rs/tokio/pull/6300

[#&#8203;6326]: https://togithub.com/tokio-rs/tokio/pull/6326

[#&#8203;6328]: https://togithub.com/tokio-rs/tokio/pull/6328

[#&#8203;6329]: https://togithub.com/tokio-rs/tokio/pull/6329

[#&#8203;6331]: https://togithub.com/tokio-rs/tokio/pull/6331

[#&#8203;6336]: https://togithub.com/tokio-rs/tokio/pull/6336

[#&#8203;6337]: https://togithub.com/tokio-rs/tokio/pull/6337

[#&#8203;6339]: https://togithub.com/tokio-rs/tokio/pull/6339

[#&#8203;6340]: https://togithub.com/tokio-rs/tokio/pull/6340

[#&#8203;6345]: https://togithub.com/tokio-rs/tokio/pull/6345

[#&#8203;6348]: https://togithub.com/tokio-rs/tokio/pull/6348

[#&#8203;6351]: https://togithub.com/tokio-rs/tokio/pull/6351

[#&#8203;6360]: https://togithub.com/tokio-rs/tokio/pull/6360

[#&#8203;6366]: https://togithub.com/tokio-rs/tokio/pull/6366

[#&#8203;6377]: https://togithub.com/tokio-rs/tokio/pull/6377

[#&#8203;6388]: https://togithub.com/tokio-rs/tokio/pull/6388

[#&#8203;6392]: https://togithub.com/tokio-rs/tokio/pull/6392

[#&#8203;6395]: https://togithub.com/tokio-rs/tokio/pull/6395

[#&#8203;6403]: https://togithub.com/tokio-rs/tokio/pull/6403

[#&#8203;6405]: https://togithub.com/tokio-rs/tokio/pull/6405

[#&#8203;6410]: https://togithub.com/tokio-rs/tokio/pull/6410

[#&#8203;6411]: https://togithub.com/tokio-rs/tokio/pull/6411

[#&#8203;6418]: https://togithub.com/tokio-rs/tokio/pull/6418

[#&#8203;6419]: https://togithub.com/tokio-rs/tokio/pull/6419

[#&#8203;6431]: https://togithub.com/tokio-rs/tokio/pull/6431

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about these updates again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4yNjkuMiIsInVwZGF0ZWRJblZlciI6IjM3LjI2OS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiJ9-->


---

_Label `internal` added by @renovate[bot] on 2024-04-08 03:05_

---

_Merged by @charliermarsh on 2024-04-08 13:35_

---

_Closed by @charliermarsh on 2024-04-08 13:35_

---

_Branch deleted on 2024-04-08 13:35_

---
