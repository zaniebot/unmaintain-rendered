```yaml
number: 13168
title: Update Rust crate rustix to v1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/rustix-1.x
created_at: 2025-04-28T03:18:46Z
updated_at: 2025-04-28T12:49:55Z
url: https://github.com/astral-sh/uv/pull/13168
synced_at: 2026-01-10T11:10:40Z
```

# Update Rust crate rustix to v1

---

_Pull request opened by @renovate on 2025-04-28 03:18_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [rustix](https://redirect.github.com/bytecodealliance/rustix) | workspace.dependencies | major | `0.38.37` -> `1.0.0` |

---

### Release Notes

<details>
<summary>bytecodealliance/rustix (rustix)</summary>

### [`v1.0.5`](https://redirect.github.com/bytecodealliance/rustix/compare/v1.0.4...v1.0.5)

[Compare Source](https://redirect.github.com/bytecodealliance/rustix/compare/v1.0.4...v1.0.5)

### [`v1.0.4`](https://redirect.github.com/bytecodealliance/rustix/compare/v1.0.3...v1.0.4)

[Compare Source](https://redirect.github.com/bytecodealliance/rustix/compare/v1.0.3...v1.0.4)

### [`v1.0.3`](https://redirect.github.com/bytecodealliance/rustix/compare/v1.0.2...v1.0.3)

[Compare Source](https://redirect.github.com/bytecodealliance/rustix/compare/v1.0.2...v1.0.3)

### [`v1.0.2`](https://redirect.github.com/bytecodealliance/rustix/compare/v1.0.1...v1.0.2)

[Compare Source](https://redirect.github.com/bytecodealliance/rustix/compare/v1.0.1...v1.0.2)

### [`v1.0.1`](https://redirect.github.com/bytecodealliance/rustix/compare/v1.0.0...v1.0.1)

[Compare Source](https://redirect.github.com/bytecodealliance/rustix/compare/v1.0.0...v1.0.1)

### [`v1.0.0`](https://redirect.github.com/bytecodealliance/rustix/releases/tag/v1.0.0): 1.0.0

[Compare Source](https://redirect.github.com/bytecodealliance/rustix/compare/v0.38.44...v1.0.0)

This release introduces the [`Buffer` trait][Buffer trait], which is used in [`read`][read], [`pread`][pread], [`recv`][recv], [`recvfrom`][recvfrom], [`getrandom`][getrandom], [`readlinkat_raw`][readlinkat_raw], [`epoll::wait`][epoll::wait], [`kevent`][kevent], [`port::getn`][port::getn], [`getxattr`][getxattr], [`lgetxattr`][lgetxattr], [`fgetxattr`][fgetxattr], [`listxattr`][listxattr], [`llistxattr`][llistxattr], and [`flistxattr`][flistxattr], and adds support for reading data into uninitialized buffers, as well as safely reading data into the spare capacity of `Vec`s.

This release also simplifies the way network addresses are handled. Instead of having separate functions with `_v4`, `_v6`, `_unix`, `_xdp`, and now `_netlink` suffixes, rustix now uses a [`SocketAddrArg` trait][SocketAddrArg trait] so that functions such as [`bind`][bind], [`connect`][connect], [`sendto`][sendto], and [`sendmsg_addr`][sendmsg_addr] can accept any type of address, and are easier to extend to new address types in the future.

[`bind`]: https://docs.rs/rustix/1.0.0/rustix/net/fn.bind.html

[`connect`]: https://docs.rs/rustix/1.0.0/rustix/net/fn.connect.html

[`connect_unspec`]: https://docs.rs/rustix/1.0.0/rustix/net/fn.connect_unspec.html

[`sendmsg_addr`]: https://docs.rs/rustix/1.0.0/rustix/net/fn.sendmsg_addr.html

[`sendmsg`]: https://docs.rs/rustix/1.0.0/rustix/net/fn.sendmsg.html

[`sendto`]: https://docs.rs/rustix/1.0.0/rustix/net/fn.sendto.html

And, this release simplifies the `ioctl` API, replacing opcode wrapper types with const generics.

This updates several APIs to add Linux 6.13 features, and raw linux-raw-sys types are no longer exposed in the public API, so it should be easier to stay up to date with new Linux releases.

And many more new features, bug fixes, and cleanups. See the [CHANGES.md file](https://redirect.github.com/bytecodealliance/rustix/blob/main/CHANGES.md#changes-from-038x-to-1x) for the full list of breaking changes.

[`read`]: https://docs.rs/rustix/1.0.0/rustix/io/fn.read.html

[`pread`]: https://docs.rs/rustix/1.0.0/rustix/io/fn.pread.html

[`recv`]: https://docs.rs/rustix/1.0.0/rustix/net/fn.recv.html

[`recvfrom`]: https://docs.rs/rustix/1.0.0/rustix/net/fn.recvfrom.html

[`getrandom`]: https://docs.rs/rustix/1.0.0/rustix/rand/fn.getrandom.html

[`readlinkat_raw`]: https://docs.rs/rustix/1.0.0/rustix/fs/fn.readlinkat_raw.html

[`epoll::wait`]: https://docs.rs/rustix/1.0.0/rustix/event/epoll/fn.wait.html

[`getxattr`]: https://docs.rs/rustix/1.0.0/rustix/fs/fn.getxattr.html

[`lgetxattr`]: https://docs.rs/rustix/1.0.0/rustix/fs/fn.lgetxattr.html

[`fgetxattr`]: https://docs.rs/rustix/1.0.0/rustix/fs/fn.fgetxattr.html

[`listxattr`]: https://docs.rs/rustix/1.0.0/rustix/fs/fn.listxattr.html

[`llistxattr`]: https://docs.rs/rustix/1.0.0/rustix/fs/fn.llistxattr.html

[`flistxattr`]: https://docs.rs/rustix/1.0.0/rustix/fs/fn.flistxattr.html

[`kevent`]: https://docs.rs/rustix/1.0.0/x86_64-unknown-freebsd/rustix/event/kqueue/fn.kevent.html

[`port::getn`]: https://docs.rs/rustix/1.0.0/x86_64-unknown-illumos/rustix/event/port/fn.getn.html

[`Buffer` trait]: https://docs.rs/rustix/1.0.0/rustix/buffer/trait.Buffer.html

[`spare_capacity`]: https://docs.rs/rustix/1.0.0/rustix/buffer/fn.spare_capacity.html

[`SocketAddrArg` trait]: https://docs.rs/rustix/1.0.0/rustix/net/trait.SocketAddrArg.html

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yNTcuMyIsInVwZGF0ZWRJblZlciI6IjM5LjI1Ny4zIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Comment by @renovate[bot] on 2025-04-28 03:18_

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
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --package rustix@0.38.44 --precise 1.0.5
    Updating crates.io index
error: failed to select a version for the requirement `rustix = "^0.38.30"`
candidate versions found which didn't match: 1.0.5
location searched: crates.io index
required by package `which v7.0.2`
    ... which satisfies dependency `which = "^7.0.0"` (locked to 7.0.2) of package `uv-trampoline-builder v0.0.1 (/tmp/renovate/repos/github/astral-sh/uv/crates/uv-trampoline-builder)`
    ... which satisfies path dependency `uv-trampoline-builder` (locked to 0.0.1) of package `uv v0.6.17 (/tmp/renovate/repos/github/astral-sh/uv/crates/uv)`

```



---

_Label `internal` added by @renovate[bot] on 2025-04-28 03:18_

---

_Closed by @konstin on 2025-04-28 09:45_

---

_Reopened by @konstin on 2025-04-28 09:45_

---

_Comment by @codspeed-hq[bot] on 2025-04-28 09:57_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/renovate%2Frustix-1.x)

### Merging #13168 will **degrade performances by 10.55%**

<sub>Comparing <code>renovate/rustix-1.x</code> (dbec56a) with <code>main</code> (cfe82dc)</sub>



### Summary

`❌ 1` regressions  
`✅ 13` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/renovate%2Frustix-1.x)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` wheelname_tag_compatibility[flyte-short-incompatible] `` | 742.2 ns | 829.7 ns | -10.55% |


---

_Merged by @charliermarsh on 2025-04-28 12:49_

---

_Closed by @charliermarsh on 2025-04-28 12:49_

---

_Branch deleted on 2025-04-28 12:49_

---
