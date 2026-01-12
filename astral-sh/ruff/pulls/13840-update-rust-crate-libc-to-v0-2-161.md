```yaml
number: 13840
title: Update Rust crate libc to v0.2.161
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/libc-0.x-lockfile
created_at: 2024-10-21T01:41:30Z
updated_at: 2024-10-21T02:03:35Z
url: https://github.com/astral-sh/ruff/pull/13840
synced_at: 2026-01-12T15:55:45Z
```

# Update Rust crate libc to v0.2.161

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [libc](https://redirect.github.com/rust-lang/libc) | workspace.dependencies | patch | `0.2.159` -> `0.2.161` |

---

### Release Notes

<details>
<summary>rust-lang/libc (libc)</summary>

### [`v0.2.161`](https://redirect.github.com/rust-lang/libc/releases/tag/0.2.161)

[Compare Source](https://redirect.github.com/rust-lang/libc/compare/0.2.160...0.2.161)

##### Fixed

-   OpenBSD: fix `FNM_PATHNAME` and `FNM_NOESCAPE` values [#&#8203;3983](https://redirect.github.com/rust-lang/libc/pull/3983)

### [`v0.2.160`](https://redirect.github.com/rust-lang/libc/releases/tag/0.2.160)

[Compare Source](https://redirect.github.com/rust-lang/libc/compare/0.2.159...0.2.160)

##### Added

-   Android: add `PR_GET_NAME` and `PR_SET_NAME` [#&#8203;3941](https://redirect.github.com/rust-lang/libc/pull/3941)
-   Apple: add `F_TRANSFEREXTENTS` [#&#8203;3925](https://redirect.github.com/rust-lang/libc/pull/3925)
-   Apple: add `mach_error_string` [#&#8203;3913](https://redirect.github.com/rust-lang/libc/pull/3913)
-   Apple: add additional `pthread` APIs [#&#8203;3846](https://redirect.github.com/rust-lang/libc/pull/3846)
-   Apple: add the `LOCAL_PEERTOKEN` socket option [#&#8203;3929](https://redirect.github.com/rust-lang/libc/pull/3929)
-   BSD: add `RTF_*`, `RTA_*`, `RTAX_*`, and `RTM_*` definitions [#&#8203;3714](https://redirect.github.com/rust-lang/libc/pull/3714)
-   Emscripten: add `AT_EACCESS` [#&#8203;3911](https://redirect.github.com/rust-lang/libc/pull/3911)
-   Emscripten: add `getgrgid`, `getgrnam`, `getgrnam_r` and `getgrgid_r` [#&#8203;3912](https://redirect.github.com/rust-lang/libc/pull/3912)
-   Emscripten: add `getpwnam_r` and `getpwuid_r` [#&#8203;3906](https://redirect.github.com/rust-lang/libc/pull/3906)
-   FreeBSD: add `POLLRDHUP` [#&#8203;3936](https://redirect.github.com/rust-lang/libc/pull/3936)
-   Haiku: add `arc4random` [#&#8203;3945](https://redirect.github.com/rust-lang/libc/pull/3945)
-   Illumos: add `ptsname_r` [#&#8203;3867](https://redirect.github.com/rust-lang/libc/pull/3867)
-   Linux: add `fanotify` interfaces [#&#8203;3695](https://redirect.github.com/rust-lang/libc/pull/3695)
-   Linux: add `tcp_info` [#&#8203;3480](https://redirect.github.com/rust-lang/libc/pull/3480)
-   Linux: add additional AF_PACKET options [#&#8203;3540](https://redirect.github.com/rust-lang/libc/pull/3540)
-   Linux: make Elf constants always available [#&#8203;3938](https://redirect.github.com/rust-lang/libc/pull/3938)
-   Musl x86: add `iopl` and `ioperm` [#&#8203;3720](https://redirect.github.com/rust-lang/libc/pull/3720)
-   Musl: add `posix_spawn` chdir functions [#&#8203;3949](https://redirect.github.com/rust-lang/libc/pull/3949)
-   Musl: add `utmpx.h` constants [#&#8203;3908](https://redirect.github.com/rust-lang/libc/pull/3908)
-   NetBSD: add `sysctlnametomib`, `CLOCK_THREAD_CPUTIME_ID` and `CLOCK_PROCESS_CPUTIME_ID` [#&#8203;3927](https://redirect.github.com/rust-lang/libc/pull/3927)
-   Nuttx: initial support [#&#8203;3909](https://redirect.github.com/rust-lang/libc/pull/3909)
-   RTEMS: add `getentropy` [#&#8203;3973](https://redirect.github.com/rust-lang/libc/pull/3973)
-   RTEMS: initial support [#&#8203;3866](https://redirect.github.com/rust-lang/libc/pull/3866)
-   Solarish: add `POLLRDHUP`, `POSIX_FADV_*`, `O_RSYNC`, and `posix_fallocate` [#&#8203;3936](https://redirect.github.com/rust-lang/libc/pull/3936)
-   Unix: add `fnmatch.h` [#&#8203;3937](https://redirect.github.com/rust-lang/libc/pull/3937)
-   VxWorks: add riscv64 support [#&#8203;3935](https://redirect.github.com/rust-lang/libc/pull/3935)
-   VxWorks: update constants related to the scheduler  [#&#8203;3963](https://redirect.github.com/rust-lang/libc/pull/3963)

##### Changed

-   Redox: change `ino_t` to be `c_ulonglong` [#&#8203;3919](https://redirect.github.com/rust-lang/libc/pull/3919)

##### Fixed

-   ESP-IDF: fix mismatched constants and structs [#&#8203;3920](https://redirect.github.com/rust-lang/libc/pull/3920)
-   FreeBSD: fix `struct stat` on FreeBSD 12+ [#&#8203;3946](https://redirect.github.com/rust-lang/libc/pull/3946)

##### Other

-   CI: Fix CI for FreeBSD 15 [#&#8203;3950](https://redirect.github.com/rust-lang/libc/pull/3950)
-   Docs: link to `windows-sys` [#&#8203;3915](https://redirect.github.com/rust-lang/libc/pull/3915)

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC4xMjAuMSIsInVwZGF0ZWRJblZlciI6IjM4LjEyMC4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-10-21 01:41_

---

_Merged by @charliermarsh on 2024-10-21 01:49_

---

_Closed by @charliermarsh on 2024-10-21 01:49_

---

_Branch deleted on 2024-10-21 01:49_

---

_Comment by @github-actions[bot] on 2024-10-21 02:03_

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
