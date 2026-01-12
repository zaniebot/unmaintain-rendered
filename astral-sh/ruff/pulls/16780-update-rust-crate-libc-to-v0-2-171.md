```yaml
number: 16780
title: Update Rust crate libc to v0.2.171
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/libc-0.x-lockfile
created_at: 2025-03-17T02:27:05Z
updated_at: 2025-03-17T08:03:20Z
url: https://github.com/astral-sh/ruff/pull/16780
synced_at: 2026-01-12T15:55:57Z
```

# Update Rust crate libc to v0.2.171

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [libc](https://redirect.github.com/rust-lang/libc) | workspace.dependencies | patch | `0.2.170` -> `0.2.171` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>rust-lang/libc (libc)</summary>

### [`v0.2.171`](https://redirect.github.com/rust-lang/libc/releases/tag/0.2.171)

[Compare Source](https://redirect.github.com/rust-lang/libc/compare/0.2.170...0.2.171)

##### Added

-   Android: Add `if_nameindex`/`if_freenameindex` support ([#&#8203;4247](https://redirect.github.com/rust-lang/libc/pull/4247))
-   Apple: Add missing proc types and constants ([#&#8203;4310](https://redirect.github.com/rust-lang/libc/pull/4310))
-   BSD: Add `devname` ([#&#8203;4285](https://redirect.github.com/rust-lang/libc/pull/4285))
-   Cygwin: Add PTY and group API ([#&#8203;4309](https://redirect.github.com/rust-lang/libc/pull/4309))
-   Cygwin: Add support ([#&#8203;4279](https://redirect.github.com/rust-lang/libc/pull/4279))
-   FreeBSD: Make `spawn.h` interfaces available on all FreeBSD-like systems ([#&#8203;4294](https://redirect.github.com/rust-lang/libc/pull/4294))
-   Linux: Add `AF_XDP` structs for all Linux environments ([#&#8203;4163](https://redirect.github.com/rust-lang/libc/pull/4163))
-   Linux: Add SysV semaphore constants ([#&#8203;4286](https://redirect.github.com/rust-lang/libc/pull/4286))
-   Linux: Add `F_SEAL_EXEC` ([#&#8203;4316](https://redirect.github.com/rust-lang/libc/pull/4316))
-   Linux: Add `SO_PREFER_BUSY_POLL` and `SO_BUSY_POLL_BUDGET` ([#&#8203;3917](https://redirect.github.com/rust-lang/libc/pull/3917))
-   Linux: Add `devmem` structs ([#&#8203;4299](https://redirect.github.com/rust-lang/libc/pull/4299))
-   Linux: Add socket constants up to `SO_DEVMEM_DONTNEED` ([#&#8203;4299](https://redirect.github.com/rust-lang/libc/pull/4299))
-   NetBSD, OpenBSD, DragonflyBSD: Add `closefrom` ([#&#8203;4290](https://redirect.github.com/rust-lang/libc/pull/4290))
-   NuttX: Add `pw_passwd` field to `passwd` ([#&#8203;4222](https://redirect.github.com/rust-lang/libc/pull/4222))
-   Solarish: define `IP_BOUND_IF` and `IPV6_BOUND_IF` ([#&#8203;4287](https://redirect.github.com/rust-lang/libc/pull/4287))
-   Wali: Add bindings for `wasm32-wali-linux-musl` target ([#&#8203;4244](https://redirect.github.com/rust-lang/libc/pull/4244))

##### Changed

-   AIX: Use `sa_sigaction` instead of a union ([#&#8203;4250](https://redirect.github.com/rust-lang/libc/pull/4250))
-   Make `msqid_ds.__msg_cbytes` public ([#&#8203;4301](https://redirect.github.com/rust-lang/libc/pull/4301))
-   Unix: Make all `major`, `minor`, `makedev` into `const fn` ([#&#8203;4208](https://redirect.github.com/rust-lang/libc/pull/4208))

##### Deprecated

-   Linux: Deprecate obsolete packet filter interfaces ([#&#8203;4267](https://redirect.github.com/rust-lang/libc/pull/4267))

##### Fixed

-   Cygwin: Fix strerror_r ([#&#8203;4308](https://redirect.github.com/rust-lang/libc/pull/4308))
-   Cygwin: Fix usage of f! ([#&#8203;4308](https://redirect.github.com/rust-lang/libc/pull/4308))
-   Hermit: Make `stat::st_size` signed ([#&#8203;4298](https://redirect.github.com/rust-lang/libc/pull/4298))
-   Linux: Correct values for `SI_TIMER`, `SI_MESGQ`, `SI_ASYNCIO` ([#&#8203;4292](https://redirect.github.com/rust-lang/libc/pull/4292))
-   NuttX: Update `tm_zone` and `d_name` fields to use `c_char` type ([#&#8203;4222](https://redirect.github.com/rust-lang/libc/pull/4222))
-   Xous: Include the prelude to define `c_int` ([#&#8203;4304](https://redirect.github.com/rust-lang/libc/pull/4304))

##### Other

-   Add labels to FIXMEs ([#&#8203;4231](https://redirect.github.com/rust-lang/libc/pull/4231), [#&#8203;4232](https://redirect.github.com/rust-lang/libc/pull/4232), [#&#8203;4234](https://redirect.github.com/rust-lang/libc/pull/4234), [#&#8203;4235](https://redirect.github.com/rust-lang/libc/pull/4235), [#&#8203;4236](https://redirect.github.com/rust-lang/libc/pull/4236))
-   CI: Fix "cannot find libc" error on Sparc64 ([#&#8203;4317](https://redirect.github.com/rust-lang/libc/pull/4317))
-   CI: Fix "cannot find libc" error on s390x ([#&#8203;4317](https://redirect.github.com/rust-lang/libc/pull/4317))
-   CI: Pass `--no-self-update` to `rustup update` ([#&#8203;4306](https://redirect.github.com/rust-lang/libc/pull/4306))
-   CI: Remove tests for the `i586-pc-windows-msvc` target ([#&#8203;4311](https://redirect.github.com/rust-lang/libc/pull/4311))
-   CI: Remove the `check_cfg` job ([#&#8203;4322](https://redirect.github.com/rust-lang/libc/pull/4312))
-   Change the range syntax that is giving `ctest` problems ([#&#8203;4311](https://redirect.github.com/rust-lang/libc/pull/4311))
-   Linux: Split out the stat struct for gnu/b32/mips ([#&#8203;4276](https://redirect.github.com/rust-lang/libc/pull/4276))

##### Removed

-   NuttX: Remove `pthread_set_name_np` ([#&#8203;4251](https://redirect.github.com/rust-lang/libc/pull/4251))

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yMDAuMCIsInVwZGF0ZWRJblZlciI6IjM5LjIwMC4wIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-03-17 02:27_

---

_Merged by @MichaReiser on 2025-03-17 07:53_

---

_Closed by @MichaReiser on 2025-03-17 07:53_

---

_Branch deleted on 2025-03-17 07:53_

---

_Comment by @github-actions[bot] on 2025-03-17 08:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
