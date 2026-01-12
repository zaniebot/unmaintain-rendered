```yaml
number: 19960
title: Update Rust crate libc to v0.2.175
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/libc-0.x-lockfile
created_at: 2025-08-18T02:07:36Z
updated_at: 2025-08-18T06:53:33Z
url: https://github.com/astral-sh/ruff/pull/19960
synced_at: 2026-01-12T15:56:51Z
```

# Update Rust crate libc to v0.2.175

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [libc](https://redirect.github.com/rust-lang/libc) | workspace.dependencies | patch | `0.2.174` -> `0.2.175` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>rust-lang/libc (libc)</summary>

### [`v0.2.175`](https://redirect.github.com/rust-lang/libc/releases/tag/0.2.175)

[Compare Source](https://redirect.github.com/rust-lang/libc/compare/0.2.174...0.2.175)

##### Added

- AIX: Add `getpeereid` ([#&#8203;4524](https://redirect.github.com/rust-lang/libc/pull/4524))
- AIX: Add `struct ld_info` and friends ([#&#8203;4578](https://redirect.github.com/rust-lang/libc/pull/4578))
- AIX: Retore `struct winsize` ([#&#8203;4577](https://redirect.github.com/rust-lang/libc/pull/4577))
- Android: Add UDP socket option constants ([#&#8203;4619](https://redirect.github.com/rust-lang/libc/pull/4619))
- Android: Add `CLONE_CLEAR_SIGHAND` and `CLONE_INTO_CGROUP` ([#&#8203;4502](https://redirect.github.com/rust-lang/libc/pull/4502))
- Android: Add more `prctl` constants ([#&#8203;4531](https://redirect.github.com/rust-lang/libc/pull/4531))
- FreeBSD Add further TCP stack-related constants ([#&#8203;4196](https://redirect.github.com/rust-lang/libc/pull/4196))
- FreeBSD x86-64: Add `mcontext_t.mc_tlsbase ` ([#&#8203;4503](https://redirect.github.com/rust-lang/libc/pull/4503))
- FreeBSD15: Add `kinfo_proc.ki_uerrmsg` ([#&#8203;4552](https://redirect.github.com/rust-lang/libc/pull/4552))
- FreeBSD: Add `in_conninfo` ([#&#8203;4482](https://redirect.github.com/rust-lang/libc/pull/4482))
- FreeBSD: Add `xinpgen` and related types ([#&#8203;4482](https://redirect.github.com/rust-lang/libc/pull/4482))
- FreeBSD: Add `xktls_session` ([#&#8203;4482](https://redirect.github.com/rust-lang/libc/pull/4482))
- Haiku: Add functionality from `libbsd` ([#&#8203;4221](https://redirect.github.com/rust-lang/libc/pull/4221))
- Linux: Add `SECBIT_*` ([#&#8203;4480](https://redirect.github.com/rust-lang/libc/pull/4480))
- NetBSD, OpenBSD: Export `ioctl` request generator macros ([#&#8203;4460](https://redirect.github.com/rust-lang/libc/pull/4460))
- NetBSD: Add `ptsname_r` ([#&#8203;4608](https://redirect.github.com/rust-lang/libc/pull/4608))
- RISCV32: Add time-related syscalls ([#&#8203;4612](https://redirect.github.com/rust-lang/libc/pull/4612))
- Solarish: Add `strftime*` ([#&#8203;4453](https://redirect.github.com/rust-lang/libc/pull/4453))
- linux: Add `EXEC_RESTRICT_*` and `EXEC_DENY_*` ([#&#8203;4545](https://redirect.github.com/rust-lang/libc/pull/4545))

##### Changed

- AIX: Add `const` to signatures to be consistent with other platforms ([#&#8203;4563](https://redirect.github.com/rust-lang/libc/pull/4563))

##### Fixed

- AIX: Fix the type of `struct statvfs.f_fsid` ([#&#8203;4576](https://redirect.github.com/rust-lang/libc/pull/4576))
- AIX: Fix the type of constants for the `ioctl` `request` argument ([#&#8203;4582](https://redirect.github.com/rust-lang/libc/pull/4582))
- AIX: Fix the types of `stat{,64}.st_*tim` ([#&#8203;4597](https://redirect.github.com/rust-lang/libc/pull/4597))
- AIX: Use unique `errno` values ([#&#8203;4507](https://redirect.github.com/rust-lang/libc/pull/4507))
- Build: Fix an incorrect `target_os` -> `target_arch` check ([#&#8203;4550](https://redirect.github.com/rust-lang/libc/pull/4550))
- FreeBSD: Fix the type of `xktls_session_onedir.ifnet` ([#&#8203;4552](https://redirect.github.com/rust-lang/libc/pull/4552))
- Mips64 musl: Fix the type of `nlink_t` ([#&#8203;4509](https://redirect.github.com/rust-lang/libc/pull/4509))
- Mips64 musl: Use a special MIPS definition of `stack_t` ([#&#8203;4528](https://redirect.github.com/rust-lang/libc/pull/4528))
- Mips64: Fix `SI_TIMER`, `SI_MESGQ` and `SI_ASYNCIO` definitions ([#&#8203;4529](https://redirect.github.com/rust-lang/libc/pull/4529))
- Musl Mips64: Swap the order of `si_errno` and `si_code` in `siginfo_t` ([#&#8203;4530](https://redirect.github.com/rust-lang/libc/pull/4530))
- Musl Mips64: Use a special MIPS definition of `statfs` ([#&#8203;4527](https://redirect.github.com/rust-lang/libc/pull/4527))
- Musl: Fix the definition of `fanotify_event_metadata` ([#&#8203;4510](https://redirect.github.com/rust-lang/libc/pull/4510))
- NetBSD: Correct `enum fae_action` to be `#[repr(C)]` ([#&#8203;60a8cfd5](https://redirect.github.com/rust-lang/libc/commit/60a8cfd564f83164d45b9533ff7a0d7371878f2a))
- PSP: Correct `char` -> `c_char` ([eaab4fc3](https://redirect.github.com/rust-lang/libc/commit/eaab4fc3f05dc646a953d4fd5ba46dfa1f8bd6f6))
- PowerPC musl: Fix `termios` definitions ([#&#8203;4518](https://redirect.github.com/rust-lang/libc/pull/4518))
- PowerPC musl: Fix the definition of `EDEADLK` ([#&#8203;4517](https://redirect.github.com/rust-lang/libc/pull/4517))
- PowerPC musl: Fix the definition of `NCCS` ([#&#8203;4513](https://redirect.github.com/rust-lang/libc/pull/4513))
- PowerPC musl: Fix the definitions of `MAP_LOCKED` and `MAP_NORESERVE` ([#&#8203;4516](https://redirect.github.com/rust-lang/libc/pull/4516))
- PowerPC64 musl: Fix the definition of `shmid_ds` ([#&#8203;4519](https://redirect.github.com/rust-lang/libc/pull/4519))

##### Deprecated

- Linux: `MAP_32BIT` is only defined on x86 on non-x86 architectures ([#&#8203;4511](https://redirect.github.com/rust-lang/libc/pull/4511))

##### Removed

- AIX: Remove duplicate constant definitions `FIND` and `ENTER` ([#&#8203;4588](https://redirect.github.com/rust-lang/libc/pull/4588))
- s390x musl: Remove `O_FSYNC` ([#&#8203;4515](https://redirect.github.com/rust-lang/libc/pull/4515))
- s390x musl: Remove `RTLD_DEEPBIND` ([#&#8203;4515](https://redirect.github.com/rust-lang/libc/pull/4515))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS43MS4xIiwidXBkYXRlZEluVmVyIjoiNDEuNzEuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-08-18 02:07_

---

_Comment by @github-actions[bot] on 2025-08-18 02:10_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-18 02:09:31.519270413 +0000
+++ new-output.txt	2025-08-18 02:09:31.586271077 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19474)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(688e)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-18 02:19_

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

_Merged by @MichaReiser on 2025-08-18 06:53_

---

_Closed by @MichaReiser on 2025-08-18 06:53_

---

_Branch deleted on 2025-08-18 06:53_

---
