```yaml
number: 17599
title: Update Rust crate tokio to v1.49.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/tokio-1.x-lockfile
created_at: 2026-01-19T01:35:15Z
updated_at: 2026-01-19T16:00:15Z
url: https://github.com/astral-sh/uv/pull/17599
synced_at: 2026-01-19T16:27:20Z
```

# Update Rust crate tokio to v1.49.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [tokio](https://tokio.rs) ([source](https://redirect.github.com/tokio-rs/tokio)) | workspace.dependencies | minor | `1.47.1` → `1.49.0` |

---

### Release Notes

<details>
<summary>tokio-rs/tokio (tokio)</summary>

### [`v1.49.0`](https://redirect.github.com/tokio-rs/tokio/releases/tag/tokio-1.49.0): Tokio v1.49.0

[Compare Source](https://redirect.github.com/tokio-rs/tokio/compare/tokio-1.48.0...tokio-1.49.0)

##### 1.49.0 (January 3rd, 2026)

##### Added

- net: add support for `TCLASS` option on IPv6 ([#&#8203;7781])
- runtime: stabilize `runtime::id::Id` ([#&#8203;7125])
- task: implement `Extend` for `JoinSet` ([#&#8203;7195])
- task: stabilize the `LocalSet::id()` ([#&#8203;7776])

##### Changed

- net: deprecate `{TcpStream,TcpSocket}::set_linger` ([#&#8203;7752])

##### Fixed

- macros: fix the hygiene issue of `join!` and `try_join!` ([#&#8203;7766])
- runtime: revert "replace manual vtable definitions with Wake" ([#&#8203;7699])
- sync: return `TryRecvError::Disconnected` from `Receiver::try_recv` after `Receiver::close` ([#&#8203;7686])
- task: remove unnecessary trait bounds on the `Debug` implementation ([#&#8203;7720])

##### Unstable

- fs: handle `EINTR` in `fs::write` for io-uring ([#&#8203;7786])
- fs: support io-uring with `tokio::fs::read` ([#&#8203;7696])
- runtime: disable io-uring on `EPERM` ([#&#8203;7724])
- time: add alternative timer for better multicore scalability ([#&#8203;7467])

##### Documented

- docs: fix a typos in `bounded.rs` and `park.rs` ([#&#8203;7817])
- io: add `SyncIoBridge` cross-references to `copy` and `copy_buf` ([#&#8203;7798])
- io: doc that `AsyncWrite` does not inherit from `std::io::Write` ([#&#8203;7705])
- metrics: clarify that `num_alive_tasks` is not strongly consistent ([#&#8203;7614])
- net: clarify the cancellation safety of the `TcpStream::peek` ([#&#8203;7305])
- net: clarify the drop behavior of `unix::OwnedWriteHalf` ([#&#8203;7742])
- net: clarify the platform-dependent backlog in `TcpSocket` docs ([#&#8203;7738])
- runtime: mention `LocalRuntime` in `new_current_thread` docs ([#&#8203;7820])
- sync: add missing period to `mpsc::Sender::try_send` docs ([#&#8203;7721])
- sync: clarify the cancellation safety of `oneshot::Receiver` ([#&#8203;7780])
- sync: improve the docs for the `errors` of mpsc ([#&#8203;7722])
- task: add example for `spawn_local` usage on local runtime ([#&#8203;7689])

[#&#8203;7125]: https://redirect.github.com/tokio-rs/tokio/pull/7125

[#&#8203;7195]: https://redirect.github.com/tokio-rs/tokio/pull/7195

[#&#8203;7305]: https://redirect.github.com/tokio-rs/tokio/pull/7305

[#&#8203;7467]: https://redirect.github.com/tokio-rs/tokio/pull/7467

[#&#8203;7614]: https://redirect.github.com/tokio-rs/tokio/pull/7614

[#&#8203;7686]: https://redirect.github.com/tokio-rs/tokio/pull/7686

[#&#8203;7689]: https://redirect.github.com/tokio-rs/tokio/pull/7689

[#&#8203;7696]: https://redirect.github.com/tokio-rs/tokio/pull/7696

[#&#8203;7699]: https://redirect.github.com/tokio-rs/tokio/pull/7699

[#&#8203;7705]: https://redirect.github.com/tokio-rs/tokio/pull/7705

[#&#8203;7720]: https://redirect.github.com/tokio-rs/tokio/pull/7720

[#&#8203;7721]: https://redirect.github.com/tokio-rs/tokio/pull/7721

[#&#8203;7722]: https://redirect.github.com/tokio-rs/tokio/pull/7722

[#&#8203;7724]: https://redirect.github.com/tokio-rs/tokio/pull/7724

[#&#8203;7738]: https://redirect.github.com/tokio-rs/tokio/pull/7738

[#&#8203;7742]: https://redirect.github.com/tokio-rs/tokio/pull/7742

[#&#8203;7752]: https://redirect.github.com/tokio-rs/tokio/pull/7752

[#&#8203;7766]: https://redirect.github.com/tokio-rs/tokio/pull/7766

[#&#8203;7776]: https://redirect.github.com/tokio-rs/tokio/pull/7776

[#&#8203;7780]: https://redirect.github.com/tokio-rs/tokio/pull/7780

[#&#8203;7781]: https://redirect.github.com/tokio-rs/tokio/pull/7781

[#&#8203;7786]: https://redirect.github.com/tokio-rs/tokio/pull/7786

[#&#8203;7798]: https://redirect.github.com/tokio-rs/tokio/pull/7798

[#&#8203;7817]: https://redirect.github.com/tokio-rs/tokio/pull/7817

[#&#8203;7820]: https://redirect.github.com/tokio-rs/tokio/pull/7820

### [`v1.48.0`](https://redirect.github.com/tokio-rs/tokio/releases/tag/tokio-1.48.0): Tokio v1.48.0

[Compare Source](https://redirect.github.com/tokio-rs/tokio/compare/tokio-1.47.3...tokio-1.48.0)

##### 1.48.0 (October 14th, 2025)

The MSRV is increased to 1.71.

##### Added

- fs: add `File::max_buf_size` ([#&#8203;7594])
- io: export `Chain` of `AsyncReadExt::chain` ([#&#8203;7599])
- net: add `SocketAddr::as_abstract_name` ([#&#8203;7491])
- net: add `TcpStream::quickack` and `TcpStream::set_quickack` ([#&#8203;7490])
- net: implement `AsRef<Self>` for `TcpStream` and `UnixStream` ([#&#8203;7573])
- task: add `LocalKey::try_get` ([#&#8203;7666])
- task: implement `Ord` for `task::Id` ([#&#8203;7530])

##### Changed

- deps: bump windows-sys to version 0.61 ([#&#8203;7645])
- fs: preserve `max_buf_size` when cloning a `File` ([#&#8203;7593])
- macros: suppress `clippy::unwrap_in_result` in `#[tokio::main]` ([#&#8203;7651])
- net: remove `PollEvented` noise from Debug formats ([#&#8203;7675])
- process: upgrade `Command::spawn_with` to use `FnOnce` ([#&#8203;7511])
- sync: remove inner mutex in `SetOnce` ([#&#8203;7554])
- sync: use `UnsafeCell::get_mut` in `Mutex::get_mut` and `RwLock::get_mut` ([#&#8203;7569])
- time: reduce the generated code size of `Timeout<T>::poll` ([#&#8203;7535])

##### Fixed

- macros: fix hygiene issue in `join!` and `try_join!` ([#&#8203;7638])
- net: fix copy/paste errors in udp peek methods ([#&#8203;7604])
- process: fix error when runtime is shut down on nightly-2025-10-12 ([#&#8203;7672])
- runtime: use release ordering in `wake_by_ref()` even if already woken ([#&#8203;7622])
- sync: close the `broadcast::Sender` in `broadcast::Sender::new()` ([#&#8203;7629])
- sync: fix implementation of unused `RwLock::try_*` methods ([#&#8203;7587])

##### Unstable

- tokio: use cargo features instead of `--cfg` flags for `taskdump` and `io_uring` ([#&#8203;7655], [#&#8203;7621])
- fs: support `io_uring` in `fs::write` ([#&#8203;7567])
- fs: support `io_uring` with `File::open()` ([#&#8203;7617])
- fs: support `io_uring` with `OpenOptions` ([#&#8203;7321])
- macros: add `local` runtime flavor ([#&#8203;7375], [#&#8203;7597])

##### Documented

- io: clarify the zero capacity case of `AsyncRead::poll_read` ([#&#8203;7580])
- io: fix typos in the docs of `AsyncFd` readiness guards ([#&#8203;7583])
- net: clarify socket gets closed on drop ([#&#8203;7526])
- net: clarify the behavior of `UCred::pid()` on Cygwin ([#&#8203;7611])
- net: clarify the supported platform of `set_reuseport()` and `reuseport()` ([#&#8203;7628])
- net: qualify that `SO_REUSEADDR` is only set on Unix ([#&#8203;7533])
- runtime: add guide for choosing between runtime types ([#&#8203;7635])
- runtime: clarify the behavior of `Handle::block_on` ([#&#8203;7665])
- runtime: clarify the edge case of `Builder::global_queue_interval()` ([#&#8203;7605])
- sync: clarify bounded channel panic behavior ([#&#8203;7641])
- sync: clarify the behavior of `tokio::sync::watch::Receiver` ([#&#8203;7584])
- sync: document cancel safety on `SetOnce::wait` ([#&#8203;7506])
- sync: fix the docs of `parking_lot` feature flag ([#&#8203;7663])
- sync: improve the docs of `UnboundedSender::send` ([#&#8203;7661])
- sync: improve the docs of `sync::watch` ([#&#8203;7601])
- sync: reword allocation failure paragraph in broadcast docs ([#&#8203;7595])
- task: clarify the behavior of several `spawn_local` methods ([#&#8203;7669])
- task: clarify the task ID reuse guarantees ([#&#8203;7577])
- task: improve the example of `poll_proceed` ([#&#8203;7586])

[#&#8203;7321]: https://redirect.github.com/tokio-rs/tokio/pull/7321

[#&#8203;7375]: https://redirect.github.com/tokio-rs/tokio/pull/7375

[#&#8203;7490]: https://redirect.github.com/tokio-rs/tokio/pull/7490

[#&#8203;7491]: https://redirect.github.com/tokio-rs/tokio/pull/7491

[#&#8203;7494]: https://redirect.github.com/tokio-rs/tokio/pull/7494

[#&#8203;7506]: https://redirect.github.com/tokio-rs/tokio/pull/7506

[#&#8203;7511]: https://redirect.github.com/tokio-rs/tokio/pull/7511

[#&#8203;7526]: https://redirect.github.com/tokio-rs/tokio/pull/7526

[#&#8203;7530]: https://redirect.github.com/tokio-rs/tokio/pull/7530

[#&#8203;7533]: https://redirect.github.com/tokio-rs/tokio/pull/7533

[#&#8203;7535]: https://redirect.github.com/tokio-rs/tokio/pull/7535

[#&#8203;7554]: https://redirect.github.com/tokio-rs/tokio/pull/7554

[#&#8203;7567]: https://redirect.github.com/tokio-rs/tokio/pull/7567

[#&#8203;7569]: https://redirect.github.com/tokio-rs/tokio/pull/7569

[#&#8203;7573]: https://redirect.github.com/tokio-rs/tokio/pull/7573

[#&#8203;7577]: https://redirect.github.com/tokio-rs/tokio/pull/7577

[#&#8203;7580]: https://redirect.github.com/tokio-rs/tokio/pull/7580

[#&#8203;7583]: https://redirect.github.com/tokio-rs/tokio/pull/7583

[#&#8203;7584]: https://redirect.github.com/tokio-rs/tokio/pull/7584

[#&#8203;7586]: https://redirect.github.com/tokio-rs/tokio/pull/7586

[#&#8203;7587]: https://redirect.github.com/tokio-rs/tokio/pull/7587

[#&#8203;7593]: https://redirect.github.com/tokio-rs/tokio/pull/7593

[#&#8203;7594]: https://redirect.github.com/tokio-rs/tokio/pull/7594

[#&#8203;7595]: https://redirect.github.com/tokio-rs/tokio/pull/7595

[#&#8203;7597]: https://redirect.github.com/tokio-rs/tokio/pull/7597

[#&#8203;7599]: https://redirect.github.com/tokio-rs/tokio/pull/7599

[#&#8203;7601]: https://redirect.github.com/tokio-rs/tokio/pull/7601

[#&#8203;7604]: https://redirect.github.com/tokio-rs/tokio/pull/7604

[#&#8203;7605]: https://redirect.github.com/tokio-rs/tokio/pull/7605

[#&#8203;7611]: https://redirect.github.com/tokio-rs/tokio/pull/7611

[#&#8203;7617]: https://redirect.github.com/tokio-rs/tokio/pull/7617

[#&#8203;7621]: https://redirect.github.com/tokio-rs/tokio/pull/7621

[#&#8203;7622]: https://redirect.github.com/tokio-rs/tokio/pull/7622

[#&#8203;7628]: https://redirect.github.com/tokio-rs/tokio/pull/7628

[#&#8203;7629]: https://redirect.github.com/tokio-rs/tokio/pull/7629

[#&#8203;7635]: https://redirect.github.com/tokio-rs/tokio/pull/7635

[#&#8203;7638]: https://redirect.github.com/tokio-rs/tokio/pull/7638

[#&#8203;7641]: https://redirect.github.com/tokio-rs/tokio/pull/7641

[#&#8203;7645]: https://redirect.github.com/tokio-rs/tokio/pull/7645

[#&#8203;7651]: https://redirect.github.com/tokio-rs/tokio/pull/7651

[#&#8203;7655]: https://redirect.github.com/tokio-rs/tokio/pull/7655

[#&#8203;7661]: https://redirect.github.com/tokio-rs/tokio/pull/7661

[#&#8203;7663]: https://redirect.github.com/tokio-rs/tokio/pull/7663

[#&#8203;7665]: https://redirect.github.com/tokio-rs/tokio/pull/7665

[#&#8203;7666]: https://redirect.github.com/tokio-rs/tokio/pull/7666

[#&#8203;7669]: https://redirect.github.com/tokio-rs/tokio/pull/7669

[#&#8203;7672]: https://redirect.github.com/tokio-rs/tokio/pull/7672

[#&#8203;7675]: https://redirect.github.com/tokio-rs/tokio/pull/7675

### [`v1.47.3`](https://redirect.github.com/tokio-rs/tokio/compare/tokio-1.47.2...tokio-1.47.3)

[Compare Source](https://redirect.github.com/tokio-rs/tokio/compare/tokio-1.47.2...tokio-1.47.3)

### [`v1.47.2`](https://redirect.github.com/tokio-rs/tokio/compare/tokio-1.47.1...tokio-1.47.2)

[Compare Source](https://redirect.github.com/tokio-rs/tokio/compare/tokio-1.47.1...tokio-1.47.2)

</details>

---

### Configuration

📅 **Schedule**: Branch creation - Between 12:00 AM and 03:59 AM, only on Monday ( * 0-3 * * 1 ) (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi43NC41IiwidXBkYXRlZEluVmVyIjoiNDIuNzQuNSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-19 01:35_

---

_Merged by @zanieb on 2026-01-19 16:00_

---

_Closed by @zanieb on 2026-01-19 16:00_

---

_Branch deleted on 2026-01-19 16:00_

---
