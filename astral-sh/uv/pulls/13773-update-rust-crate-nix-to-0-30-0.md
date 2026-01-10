```yaml
number: 13773
title: Update Rust crate nix to 0.30.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/nix-0.x
created_at: 2025-06-02T00:56:08Z
updated_at: 2025-06-02T08:05:02Z
url: https://github.com/astral-sh/uv/pull/13773
synced_at: 2026-01-10T11:10:42Z
```

# Update Rust crate nix to 0.30.0

---

_Pull request opened by @renovate on 2025-06-02 00:56_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [nix](https://redirect.github.com/nix-rust/nix) | workspace.dependencies | minor | `0.29.0` -> `0.30.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>nix-rust/nix (nix)</summary>

### [`v0.30.1`](https://redirect.github.com/nix-rust/nix/blob/HEAD/CHANGELOG.md#0301---2025-05-04)

[Compare Source](https://redirect.github.com/nix-rust/nix/compare/v0.30.0...v0.30.1)

##### Fixed

-   doc.rs build
    ([#&#8203;2634](https://redirect.github.com/nix-rust/nix/pull/2634))

### [`v0.30.0`](https://redirect.github.com/nix-rust/nix/blob/HEAD/CHANGELOG.md#0300---2025-04-29)

[Compare Source](https://redirect.github.com/nix-rust/nix/compare/v0.29.0...v0.30.0)

##### Added

-   Add socket option `IPV6_PKTINFO` for BSDs/Linux/Android, also
    `IPV6_RECVPKTINFO` for DragonFlyBSD
    ([#&#8203;2113](https://redirect.github.com/nix-rust/nix/pull/2113))
-   Add `fcntl`'s `F_PREALLOCATE` constant for Apple targets.
    ([#&#8203;2393](https://redirect.github.com/nix-rust/nix/pull/2393))
-   Improve support for extracting the TTL / Hop Limit from incoming packets
    and support for DSCP (ToS / Traffic Class).
    ([#&#8203;2425](https://redirect.github.com/nix-rust/nix/pull/2425))
-   Add socket option IP_TOS (nix::sys::socket::sockopt::IpTos) IPV6\_TCLASS
    (nix::sys::socket::sockopt::Ipv6TClass) on Android/FreeBSD
    ([#&#8203;2464](https://redirect.github.com/nix-rust/nix/pull/2464))
-   Add `SeekData` and `SeekHole` to `Whence` for hurd and apple targets
    ([#&#8203;2473](https://redirect.github.com/nix-rust/nix/pull/2473))
-   Add `From` trait implementation between `SocketAddr` and `Sockaddr`,
    `Sockaddr6` ([#&#8203;2474](https://redirect.github.com/nix-rust/nix/pull/2474))
-   Added wrappers for `posix_spawn` API
    ([#&#8203;2475](https://redirect.github.com/nix-rust/nix/pull/2475))
-   Add the support for Emscripten.
    ([#&#8203;2477](https://redirect.github.com/nix-rust/nix/pull/2477))
-   Add fcntl constant `F_RDADVISE` for Apple target
    ([#&#8203;2480](https://redirect.github.com/nix-rust/nix/pull/2480))
-   Add fcntl constant `F_RDAHEAD` for Apple target
    ([#&#8203;2482](https://redirect.github.com/nix-rust/nix/pull/2482))
-   Add `F_LOG2PHYS` and `F_LOG2PHYS_EXT` for Apple target
    ([#&#8203;2483](https://redirect.github.com/nix-rust/nix/pull/2483))
-   `MAP_SHARED_VALIDATE` was added for all linux targets. & `MAP_SYNC` was added
    for linux with the exclusion of mips architecures, and uclibc
    ([#&#8203;2499](https://redirect.github.com/nix-rust/nix/pull/2499))
-   Add `getregs()`/`getregset()`/`setregset()` for Linux/musl/aarch64
    ([#&#8203;2502](https://redirect.github.com/nix-rust/nix/pull/2502))
-   Add FcntlArgs `F_TRANSFEREXTENTS` constant for Apple targets
    ([#&#8203;2504](https://redirect.github.com/nix-rust/nix/pull/2504))
-   Add `MapFlags::MAP_STACK` in `sys::man` for netbsd
    ([#&#8203;2526](https://redirect.github.com/nix-rust/nix/pull/2526))
-   Add support for `libc::LOCAL_PEERTOKEN` in `getsockopt`.
    ([#&#8203;2529](https://redirect.github.com/nix-rust/nix/pull/2529))
-   Add support for `syslog`, `openlog`, `closelog` on all `unix`.
    ([#&#8203;2537](https://redirect.github.com/nix-rust/nix/pull/2537))
-   Add the `TCP_FUNCTION_BLK` sockopt, on FreeBSD.
    ([#&#8203;2539](https://redirect.github.com/nix-rust/nix/pull/2539))
-   Implements `Into<OwnedFd>` for `PtyMaster/Fanotify/Inotify/SignalFd/TimerFd`
    ([#&#8203;2548](https://redirect.github.com/nix-rust/nix/pull/2548))
-   Add `MremapFlags::MREMAP_DONTUNMAP` to `sys::mman::mremap` for linux target.
    ([#&#8203;2555](https://redirect.github.com/nix-rust/nix/pull/2555))
-   Added `sockopt_impl!` to the public API.  It's now possible for users to
    define
    their own sockopts without needing to make a PR to Nix.
    ([#&#8203;2556](https://redirect.github.com/nix-rust/nix/pull/2556))
-   Add the `TCP_FUNCTION_ALIAS` sockopt, on FreeBSD.
    ([#&#8203;2558](https://redirect.github.com/nix-rust/nix/pull/2558))
-   Add `sys::mman::MmapAdvise` `MADV_PAGEOUT`, `MADV_COLD`, `MADV_WIPEONFORK`,
    `MADV_KEEPONFORK` for Linux and Android targets
    ([#&#8203;2559](https://redirect.github.com/nix-rust/nix/pull/2559))
-   Add socket protocol `Sctp`, as well as `MSG_NOTIFICATION` for non-Android
    Linux targets. ([#&#8203;2562](https://redirect.github.com/nix-rust/nix/pull/2562))
-   Added `from_owned_fd` constructor to `EventFd`
    ([#&#8203;2563](https://redirect.github.com/nix-rust/nix/pull/2563))
-   Add `sys::mman::MmapAdvise` `MADV_POPULATE_READ`, `MADV_POPULATE_WRITE` for
    Linux and Android targets
    ([#&#8203;2565](https://redirect.github.com/nix-rust/nix/pull/2565))
-   Added `from_owned_fd` constructor to
    `PtyMaster/Fanotify/Inotify/SignalFd/TimerFd`
    ([#&#8203;2566](https://redirect.github.com/nix-rust/nix/pull/2566))
-   Added `FcntlArg::F_READAHEAD` for FreeBSD target
    ([#&#8203;2569](https://redirect.github.com/nix-rust/nix/pull/2569))
-   Added `sockopt::LingerSec` for Apple targets
    ([#&#8203;2572](https://redirect.github.com/nix-rust/nix/pull/2572))
-   Added `sockopt::EsclBind` for solarish targets
    ([#&#8203;2573](https://redirect.github.com/nix-rust/nix/pull/2573))
-   Exposed the `std::os::fd::AsRawFd` trait method for
    `nix::sys::fanotify::Fanotify` struct
    ([#&#8203;2575](https://redirect.github.com/nix-rust/nix/pull/2575))
-   Add support for syslog's `setlogmask` on all `unix`.
    ([#&#8203;2579](https://redirect.github.com/nix-rust/nix/pull/2579))
-   Added Fuchsia support for `ioctl`.
    ([#&#8203;2580](https://redirect.github.com/nix-rust/nix/pull/2580))
-   Add `sys::socket::SockProtocol::EthIp`,
    `sys::socket::SockProtocol::EthIpv6`,
    `sys::socket::SockProtocol::EthLoop`
    ([#&#8203;2581](https://redirect.github.com/nix-rust/nix/pull/2581))
-   Add OpenHarmony target into CI and Update documents.
    ([#&#8203;2599](https://redirect.github.com/nix-rust/nix/pull/2599))
-   Added the TcpMaxSeg `setsockopt` option for apple targets
    ([#&#8203;2603](https://redirect.github.com/nix-rust/nix/pull/2603))
-   Add `FilAttach` and `FilDetach` to socket::sockopt for Illumos
    ([#&#8203;2611](https://redirect.github.com/nix-rust/nix/pull/2611))
-   Add `PeerPidfd` (`SO_PEERPIDFD`) to `socket::sockopt` for Linux
    ([#&#8203;2620](https://redirect.github.com/nix-rust/nix/pull/2620))
-   Added `socket::sockopt::AttachReusePortCbpf` for Linux
    ([#&#8203;2621](https://redirect.github.com/nix-rust/nix/pull/2621))
-   Add `ptrace::syscall_info` for linux/glibc
    ([#&#8203;2627](https://redirect.github.com/nix-rust/nix/pull/2627))

##### Changed

-   Module sys/signal now adopts I/O safety
    ([#&#8203;1936](https://redirect.github.com/nix-rust/nix/pull/1936))
-   Change the type of the `name` argument of `memfd_create()` from `&CStr` to
    `<P: NixPath>(name: &P)` ([#&#8203;2431](https://redirect.github.com/nix-rust/nix/pull/2431))
-   Public interfaces in `fcntl.rs` and `dir.rs` now use I/O-safe types.
    ([#&#8203;2434](https://redirect.github.com/nix-rust/nix/pull/2434))
-   Module `sys/stat` now adopts I/O safety.
    ([#&#8203;2439](https://redirect.github.com/nix-rust/nix/pull/2439))
-   Module unistd now adopts I/O safety.
    ([#&#8203;2440](https://redirect.github.com/nix-rust/nix/pull/2440))
-   Module sys/fanotify now adopts I/O safety
    ([#&#8203;2443](https://redirect.github.com/nix-rust/nix/pull/2443))
-   Socket option `IpTos` has been renamed to `Ipv4Tos`, the old symbol is
    deprecated since 0.30.0 ([#&#8203;2465](https://redirect.github.com/nix-rust/nix/pull/2465))
-   Rename Flags `EventFlag` to `EvFlags`, and `MemFdCreateFlag` to `MFdFlags`
    ([#&#8203;2476](https://redirect.github.com/nix-rust/nix/pull/2476))
-   Made `nix::sys::socket::UnknownCmsg` public and more readable
    ([#&#8203;2520](https://redirect.github.com/nix-rust/nix/pull/2520))
-   recvmsg: take slice for cmsg_buffer instead of Vec
    ([#&#8203;2524](https://redirect.github.com/nix-rust/nix/pull/2524))
-   linkat: allow distinct types for path arguments
    ([#&#8203;2582](https://redirect.github.com/nix-rust/nix/pull/2582))

##### Fixed

-   Disable unsupported signals on sparc-linux
    ([#&#8203;2454](https://redirect.github.com/nix-rust/nix/pull/2454))
-   Fix cmsg_len() return type on OpenHarmony
    ([#&#8203;2456](https://redirect.github.com/nix-rust/nix/pull/2456))
-   The `ns` argument of `sys::prctl::set_timerslack()` should be of type
    `c_ulong` ([#&#8203;2505](https://redirect.github.com/nix-rust/nix/pull/2505))
-   Properly exclude NUL characters from `OSString`s returned by `getsockopt`.
    ([#&#8203;2557](https://redirect.github.com/nix-rust/nix/pull/2557))
-   Fixes the build on OpenHarmony
    ([#&#8203;2587](https://redirect.github.com/nix-rust/nix/pull/2587))

##### Removed

-   Type `SigevNotify` is no longer `PartialEq`, `Eq` and `Hash` due to the use
    of `BorrowedFd` ([#&#8203;1936](https://redirect.github.com/nix-rust/nix/pull/1936))
-   `EventFd::defuse()` is removed because it does nothing, `EventFd::arm()` is
    also removed for symmetry reasons.
    ([#&#8203;2452](https://redirect.github.com/nix-rust/nix/pull/2452))
-   Removed the `Copy` trait from `PollFd`
    ([#&#8203;2631](https://redirect.github.com/nix-rust/nix/pull/2631))

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC4zMy42IiwidXBkYXRlZEluVmVyIjoiNDAuMzMuNiIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-06-02 00:56_

---

_Comment by @renovate[bot] on 2025-06-02 00:56_

### ⚠️ Artifact update problem

Renovate failed to update an artifact related to this branch. You probably do not want to merge this PR as-is.

♻ Renovate will retry this branch, including artifacts, only when one of the following happens:

 - any of the package files in this branch needs updating, or 
 - the branch becomes conflicted, or
 - you click the rebase/retry checkbox if found above, or
 - you rename this PR's title to start with "rebase!" to trigger it manually

The artifact failure details are included below:

##### File name: Cargo.lock

```
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --package nix@0.29.0 --precise 0.30.1
    Updating crates.io index
error: failed to select a version for the requirement `nix = "^0.29"`
candidate versions found which didn't match: 0.30.1
location searched: crates.io index
required by package `homedir v0.3.4`
    ... which satisfies dependency `homedir = "^0.3.3"` (locked to 0.3.4) of package `axoupdater v0.9.0`
    ... which satisfies dependency `axoupdater = "^0.9.0"` (locked to 0.9.0) of package `uv v0.7.9 (/tmp/renovate/repos/github/astral-sh/uv/crates/uv)`

```



---

_Merged by @konstin on 2025-06-02 08:05_

---

_Closed by @konstin on 2025-06-02 08:05_

---

_Branch deleted on 2025-06-02 08:05_

---
