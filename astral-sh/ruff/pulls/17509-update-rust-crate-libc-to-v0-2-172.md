```yaml
number: 17509
title: Update Rust crate libc to v0.2.172
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/libc-0.x-lockfile
created_at: 2025-04-21T01:35:40Z
updated_at: 2025-04-21T01:51:52Z
url: https://github.com/astral-sh/ruff/pull/17509
synced_at: 2026-01-10T19:33:02Z
```

# Update Rust crate libc to v0.2.172

---

_Pull request opened by @renovate on 2025-04-21 01:35_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [libc](https://redirect.github.com/rust-lang/libc) | workspace.dependencies | patch | `0.2.171` -> `0.2.172` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>rust-lang/libc (libc)</summary>

### [`v0.2.172`](https://redirect.github.com/rust-lang/libc/releases/tag/0.2.172)

[Compare Source](https://redirect.github.com/rust-lang/libc/compare/0.2.171...0.2.172)

##### Added

-   Android: Add `getauxval` for 32-bit targets ([#&#8203;4338](https://redirect.github.com/rust-lang/libc/pull/4338))
-   Android: Add `if_tun.h` ioctls ([#&#8203;4379](https://redirect.github.com/rust-lang/libc/pull/4379))
-   Android: Define `SO_BINDTOIFINDEX` ([#&#8203;4391](https://redirect.github.com/rust-lang/libc/pull/4391))
-   Cygwin: Add `posix_spawn_file_actions_add[f]chdir[_np]` ([#&#8203;4387](https://redirect.github.com/rust-lang/libc/pull/4387))
-   Cygwin: Add new socket options ([#&#8203;4350](https://redirect.github.com/rust-lang/libc/pull/4350))
-   Cygwin: Add statfs & fcntl ([#&#8203;4321](https://redirect.github.com/rust-lang/libc/pull/4321))
-   FreeBSD: Add `filedesc` and `fdescenttbl` ([#&#8203;4327](https://redirect.github.com/rust-lang/libc/pull/4327))
-   Glibc: Add unstable support for \_FILE_OFFSET_BITS=64 ([#&#8203;4345](https://redirect.github.com/rust-lang/libc/pull/4345))
-   Hermit: Add `AF_UNSPEC` ([#&#8203;4344](https://redirect.github.com/rust-lang/libc/pull/4344))
-   Hermit: Add `AF_VSOCK` ([#&#8203;4344](https://redirect.github.com/rust-lang/libc/pull/4344))
-   Illumos, NetBSD: Add `timerfd` APIs ([#&#8203;4333](https://redirect.github.com/rust-lang/libc/pull/4333))
-   Linux: Add `_IO`, `_IOW`, `_IOR`, `_IOWR` to the exported API ([#&#8203;4325](https://redirect.github.com/rust-lang/libc/pull/4325))
-   Linux: Add `tcp_info` to uClibc bindings ([#&#8203;4347](https://redirect.github.com/rust-lang/libc/pull/4347))
-   Linux: Add further BPF program flags ([#&#8203;4356](https://redirect.github.com/rust-lang/libc/pull/4356))
-   Linux: Add missing INPUT_PROP_XXX flags from `input-event-codes.h` ([#&#8203;4326](https://redirect.github.com/rust-lang/libc/pull/4326))
-   Linux: Add missing TLS bindings ([#&#8203;4296](https://redirect.github.com/rust-lang/libc/pull/4296))
-   Linux: Add more constants from `seccomp.h` ([#&#8203;4330](https://redirect.github.com/rust-lang/libc/pull/4330))
-   Linux: Add more glibc `ptrace_sud_config` and related `PTRACE_*ET_SYSCALL_USER_DISPATCH_CONFIG`. ([#&#8203;4386](https://redirect.github.com/rust-lang/libc/pull/4386))
-   Linux: Add new netlink flags ([#&#8203;4288](https://redirect.github.com/rust-lang/libc/pull/4288))
-   Linux: Define ioctl codes on more architectures ([#&#8203;4382](https://redirect.github.com/rust-lang/libc/pull/4382))
-   Linux: Add missing `pthread_attr_setstack` ([#&#8203;4349](https://redirect.github.com/rust-lang/libc/pull/4349))
-   Musl: Add missing `utmpx` API ([#&#8203;4332](https://redirect.github.com/rust-lang/libc/pull/4332))
-   Musl: Enable `getrandom` on all platforms ([#&#8203;4346](https://redirect.github.com/rust-lang/libc/pull/4346))
-   NuttX: Add more signal constants ([#&#8203;4353](https://redirect.github.com/rust-lang/libc/pull/4353))
-   QNX: Add QNX 7.1-iosock and 8.0 to list of additional cfgs ([#&#8203;4169](https://redirect.github.com/rust-lang/libc/pull/4169))
-   QNX: Add support for alternative Neutrino network stack `io-sock` ([#&#8203;4169](https://redirect.github.com/rust-lang/libc/pull/4169))
-   Redox: Add more `sys/socket.h` and `sys/uio.h` definitions ([#&#8203;4388](https://redirect.github.com/rust-lang/libc/pull/4388))
-   Solaris: Temporarily define `O_DIRECT` and `SIGINFO` ([#&#8203;4348](https://redirect.github.com/rust-lang/libc/pull/4348))
-   Solarish: Add `secure_getenv` ([#&#8203;4342](https://redirect.github.com/rust-lang/libc/pull/4342))
-   VxWorks: Add missing `d_type` member to `dirent` ([#&#8203;4352](https://redirect.github.com/rust-lang/libc/pull/4352))
-   VxWorks: Add missing signal-related constsants ([#&#8203;4352](https://redirect.github.com/rust-lang/libc/pull/4352))
-   VxWorks: Add more error codes ([#&#8203;4337](https://redirect.github.com/rust-lang/libc/pull/4337))

##### Deprecated

-   FreeBSD: Deprecate `TCP_PCAP_OUT` and `TCP_PCAP_IN` ([#&#8203;4381](https://redirect.github.com/rust-lang/libc/pull/4381))

##### Fixed

-   Cygwin: Fix member types of `statfs` ([#&#8203;4324](https://redirect.github.com/rust-lang/libc/pull/4324))
-   Cygwin: Fix tests  ([#&#8203;4357](https://redirect.github.com/rust-lang/libc/pull/4357))
-   Hermit: Make `AF_INET = 3` ([#&#8203;4344](https://redirect.github.com/rust-lang/libc/pull/4344))
-   Musl: Fix the syscall table on RISC-V-32 ([#&#8203;4335](https://redirect.github.com/rust-lang/libc/pull/4335))
-   Musl: Fix the value of `SA_ONSTACK` on RISC-V-32 ([#&#8203;4335](https://redirect.github.com/rust-lang/libc/pull/4335))
-   VxWorks: Fix a typo in the `waitpid` parameter name ([#&#8203;4334](https://redirect.github.com/rust-lang/libc/pull/4334))

##### Removed

-   Musl: Remove `O_FSYNC` on RISC-V-32 (use `O_SYNC` instead) ([#&#8203;4335](https://redirect.github.com/rust-lang/libc/pull/4335))
-   Musl: Remove `RTLD_DEEPBIND` on RISC-V-32 ([#&#8203;4335](https://redirect.github.com/rust-lang/libc/pull/4335))

##### Other

-   CI: Add matrix env variables to the environment ([#&#8203;4345](https://redirect.github.com/rust-lang/libc/pull/4345))
-   CI: Always deny warnings ([#&#8203;4363](https://redirect.github.com/rust-lang/libc/pull/4363))
-   CI: Always upload successfully created artifacts ([#&#8203;4345](https://redirect.github.com/rust-lang/libc/pull/4345))
-   CI: Install musl from source for loongarch64 ([#&#8203;4320](https://redirect.github.com/rust-lang/libc/pull/4320))
-   CI: Revert "Also skip `MFD_EXEC` and `MFD_NOEXEC_SEAL` on sparc64" ([#]())
-   CI: Use `$PWD` instead of `$(pwd)` in run-docker ([#&#8203;4345](https://redirect.github.com/rust-lang/libc/pull/4345))
-   Solarish: Restrict `openpty` and `forkpty` polyfills to Illumos, replace Solaris implementation with bindings ([#&#8203;4329](https://redirect.github.com/rust-lang/libc/pull/4329))
-   Testing: Ensure the makedev test does not emit unused errors ([#&#8203;4363](https://redirect.github.com/rust-lang/libc/pull/4363))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yNDguNCIsInVwZGF0ZWRJblZlciI6IjM5LjI0OC40IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-04-21 01:35_

---

_Comment by @github-actions[bot] on 2025-04-21 01:45_

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

_Merged by @charliermarsh on 2025-04-21 01:51_

---

_Closed by @charliermarsh on 2025-04-21 01:51_

---

_Branch deleted on 2025-04-21 01:51_

---
