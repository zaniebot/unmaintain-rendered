```yaml
number: 14599
title: Update Rust crate tokio to v1.46.1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/tokio-1.x-lockfile
created_at: 2025-07-14T03:56:44Z
updated_at: 2025-07-14T13:58:57Z
url: https://github.com/astral-sh/uv/pull/14599
synced_at: 2026-01-10T06:53:02Z
```

# Update Rust crate tokio to v1.46.1

---

_Pull request opened by @renovate on 2025-07-14 03:56_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [tokio](https://tokio.rs) ([source](https://redirect.github.com/tokio-rs/tokio)) | dev-dependencies | minor | `1.45.1` -> `1.46.1` |
| [tokio](https://tokio.rs) ([source](https://redirect.github.com/tokio-rs/tokio)) | workspace.dependencies | minor | `1.45.1` -> `1.46.1` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>tokio-rs/tokio (tokio)</summary>

### [`v1.46.1`](https://redirect.github.com/tokio-rs/tokio/releases/tag/tokio-1.46.1): Tokio v1.46.1

[Compare Source](https://redirect.github.com/tokio-rs/tokio/compare/tokio-1.46.0...tokio-1.46.1)

### 1.46.1 (July 4th, 2025)

This release fixes incorrect spawn locations in runtime task hooks for tasks spawned using `tokio::spawn` rather than `Runtime::spawn`. This issue only effected the spawn location in `TaskMeta::spawned_at`, and did not effect task locations in Tracing events.

#### Unstable

- runtime: add `TaskMeta::spawn_location` tracking where a task was spawned ([#&#8203;7440])

[#&#8203;7440]: https://redirect.github.com/tokio-rs/tokio/pull/7440

### [`v1.46.0`](https://redirect.github.com/tokio-rs/tokio/releases/tag/tokio-1.46.0): Tokio v1.46.0

[Compare Source](https://redirect.github.com/tokio-rs/tokio/compare/tokio-1.45.1...tokio-1.46.0)

### 1.46.0 (July 2nd, 2025)

##### Fixed

- net: fixed `TcpStream::shutdown` incorrectly returning an error on macOS ([#&#8203;7290])

#### Added

- sync: `mpsc::OwnedPermit::{same_channel, same_channel_as_sender}` methods ([#&#8203;7389])
- macros: `biased` option for `join!` and `try_join!`, similar to `select!` ([#&#8203;7307])
- net: support for cygwin ([#&#8203;7393])
- net: support `pope::OpenOptions::read_write` on Android ([#&#8203;7426])
- net: add `Clone` implementation for `net::unix::SocketAddr` ([#&#8203;7422])

#### Changed

- runtime: eliminate unnecessary lfence while operating on `queue::Local<T>` ([#&#8203;7340])
- task: disallow blocking in `LocalSet::{poll,drop}` ([#&#8203;7372])

#### Unstable

- runtime: add `TaskMeta::spawn_location` tracking where a task was spawned ([#&#8203;7417])
- runtime: removed borrow from `LocalOptions` parameter to `runtime::Builder::build_local` ([#&#8203;7346])

#### Documented

- io: clarify behavior of seeking when `start_seek` is not used ([#&#8203;7366])
- io: document cancellation safety of `AsyncWriteExt::flush` ([#&#8203;7364])
- net: fix docs for `recv_buffer_size` method ([#&#8203;7336])
- net: fix broken link of `RawFd` in `TcpSocket` docs ([#&#8203;7416])
- net: update `AsRawFd` doc link to current Rust stdlib location ([#&#8203;7429])
- readme: fix double period in reactor description ([#&#8203;7363])
- runtime: add doc note that `on_*_task_poll` is unstable ([#&#8203;7311])
- sync: update broadcast docs on allocation failure ([#&#8203;7352])
- time: add a missing panic scenario of `time::advance` ([#&#8203;7394])

[#&#8203;7290]: https://redirect.github.com/tokio-rs/tokio/pull/7290

[#&#8203;7307]: https://redirect.github.com/tokio-rs/tokio/pull/7307

[#&#8203;7311]: https://redirect.github.com/tokio-rs/tokio/pull/7311

[#&#8203;7336]: https://redirect.github.com/tokio-rs/tokio/pull/7336

[#&#8203;7340]: https://redirect.github.com/tokio-rs/tokio/pull/7340

[#&#8203;7346]: https://redirect.github.com/tokio-rs/tokio/pull/7346

[#&#8203;7352]: https://redirect.github.com/tokio-rs/tokio/pull/7352

[#&#8203;7363]: https://redirect.github.com/tokio-rs/tokio/pull/7363

[#&#8203;7364]: https://redirect.github.com/tokio-rs/tokio/pull/7364

[#&#8203;7366]: https://redirect.github.com/tokio-rs/tokio/pull/7366

[#&#8203;7372]: https://redirect.github.com/tokio-rs/tokio/pull/7372

[#&#8203;7389]: https://redirect.github.com/tokio-rs/tokio/pull/7389

[#&#8203;7393]: https://redirect.github.com/tokio-rs/tokio/pull/7393

[#&#8203;7394]: https://redirect.github.com/tokio-rs/tokio/pull/7394

[#&#8203;7416]: https://redirect.github.com/tokio-rs/tokio/pull/7416

[#&#8203;7422]: https://redirect.github.com/tokio-rs/tokio/pull/7422

[#&#8203;7426]: https://redirect.github.com/tokio-rs/tokio/pull/7426

[#&#8203;7429]: https://redirect.github.com/tokio-rs/tokio/pull/7429

[#&#8203;7417]: https://redirect.github.com/tokio-rs/tokio/pull/7417

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4yMy4yIiwidXBkYXRlZEluVmVyIjoiNDEuMjMuMiIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-07-14 03:56_

---

_Merged by @charliermarsh on 2025-07-14 13:58_

---

_Closed by @charliermarsh on 2025-07-14 13:58_

---

_Branch deleted on 2025-07-14 13:58_

---
