```yaml
number: 20832
title: Update Rust crate libc to v0.2.177
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/libc-0.x-lockfile
created_at: 2025-10-13T01:07:41Z
updated_at: 2025-10-13T01:47:39Z
url: https://github.com/astral-sh/ruff/pull/20832
synced_at: 2026-01-10T17:34:34Z
```

# Update Rust crate libc to v0.2.177

---

_Pull request opened by @renovate on 2025-10-13 01:07_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [libc](https://redirect.github.com/rust-lang/libc) | workspace.dependencies | patch | `0.2.175` -> `0.2.177` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>rust-lang/libc (libc)</summary>

### [`v0.2.177`](https://redirect.github.com/rust-lang/libc/releases/tag/0.2.177)

[Compare Source](https://redirect.github.com/rust-lang/libc/compare/0.2.176...0.2.177)

##### Added

- Apple: Add `TIOCGETA`, `TIOCSETA`, `TIOCSETAW`, `TIOCSETAF` constants ([#&#8203;4736](https://redirect.github.com/rust-lang/libc/pull/4736))
- Apple: Add `pthread_cond_timedwait_relative_np` ([#&#8203;4719](https://redirect.github.com/rust-lang/libc/pull/4719))
- BSDs: Add `_CS_PATH` constant ([#&#8203;4738](https://redirect.github.com/rust-lang/libc/pull/4738))
- Linux-like: Add `SIGEMT` for mips\* and sparc\* architectures ([#&#8203;4730](https://redirect.github.com/rust-lang/libc/pull/4730))
- OpenBSD: Add `elf_aux_info` ([#&#8203;4729](https://redirect.github.com/rust-lang/libc/pull/4729))
- Redox: Add more sysconf constants ([#&#8203;4728](https://redirect.github.com/rust-lang/libc/pull/4728))
- Windows: Add `wcsnlen` ([#&#8203;4721](https://redirect.github.com/rust-lang/libc/pull/4721))

##### Changed

- WASIP2: Invert conditional to include p2 APIs ([#&#8203;4733](https://redirect.github.com/rust-lang/libc/pull/4733))

### [`v0.2.176`](https://redirect.github.com/rust-lang/libc/releases/tag/0.2.176)

[Compare Source](https://redirect.github.com/rust-lang/libc/compare/0.2.175...0.2.176)

##### Support

- The default FreeBSD version has been raised from 11 to 12. This matches `rustc` since 1.78. ([#&#8203;2406](https://redirect.github.com/rust-lang/libc/pull/2406))
- `Debug` is now always implemented, rather than being gated behind the `extra_traits` feature. ([#&#8203;4624](https://redirect.github.com/rust-lang/libc/pull/4624))

##### Added

- AIX: Restore some non-POSIX functions guarded by the `_KERNEL` macro. ([#&#8203;4607](https://redirect.github.com/rust-lang/libc/pull/4607))
- FreeBSD 14: Add `st_fileref` to `struct stat` ([#&#8203;4642](https://redirect.github.com/rust-lang/libc/pull/4642))
- Haiku: Add the `accept4` POSIX call ([#&#8203;4586](https://redirect.github.com/rust-lang/libc/pull/4586))
- Introduce a wrapper for representing padding ([#&#8203;4632](https://redirect.github.com/rust-lang/libc/pull/4632))
- Linux: Add `EM_RISCV` ([#&#8203;4659](https://redirect.github.com/rust-lang/libc/pull/4659))
- Linux: Add `MS_NOSYMFOLLOW` ([#&#8203;4389](https://redirect.github.com/rust-lang/libc/pull/4389))
- Linux: Add `backtrace_symbols(_fd)` ([#&#8203;4668](https://redirect.github.com/rust-lang/libc/pull/4668))
- Linux: Add missing `SOL_PACKET` optnames ([#&#8203;4669](https://redirect.github.com/rust-lang/libc/pull/4669))
- Musl s390x: Add `SYS_mseal` ([#&#8203;4549](https://redirect.github.com/rust-lang/libc/pull/4549))
- NuttX: Add `__errno` ([#&#8203;4687](https://redirect.github.com/rust-lang/libc/pull/4687))
- Redox: Add `dirfd`, `VDISABLE`, and resource consts ([#&#8203;4660](https://redirect.github.com/rust-lang/libc/pull/4660))
- Redox: Add more `resource.h`, `fcntl.h` constants ([#&#8203;4666](https://redirect.github.com/rust-lang/libc/pull/4666))
- Redox: Enable `strftime` and `mkostemp[s]` ([#&#8203;4629](https://redirect.github.com/rust-lang/libc/pull/4629))
- Unix, Windows: Add `qsort_r` (Unix), and `qsort(_s)` (Windows) ([#&#8203;4677](https://redirect.github.com/rust-lang/libc/pull/4677))
- Unix: Add `dlvsym` for Linux-gnu, FreeBSD, and NetBSD ([#&#8203;4671](https://redirect.github.com/rust-lang/libc/pull/4671))
- Unix: Add `sigqueue` ([#&#8203;4620](https://redirect.github.com/rust-lang/libc/pull/4620))

##### Changed

- FreeBSD 15: Mark `kinfo_proc` as non-exhaustive ([#&#8203;4553](https://redirect.github.com/rust-lang/libc/pull/4553))
- FreeBSD: Set the ELF symbol version for `readdir_r` ([#&#8203;4694](https://redirect.github.com/rust-lang/libc/pull/4694))
- Linux: Correct the config for whether or not `epoll_event` is packed ([#&#8203;4639](https://redirect.github.com/rust-lang/libc/pull/4639))
- Tests: Replace the old `ctest` with the much more reliable new implementation ([#&#8203;4655](https://redirect.github.com/rust-lang/libc/pull/4655) and many related PRs)

##### Fixed

- AIX: Fix the type of the 4th arguement of `getgrnam_r` (\[[#&#8203;4656](https://redirect.github.com/rust-lang/libc/issues/4656)]\([#&#8203;4656](https://redirect.github.com/rust-lang/libc/pull/4656)
- FreeBSD: Limit `P_IDLEPROC` to FreeBSD 15 ([#&#8203;4640](https://redirect.github.com/rust-lang/libc/pull/4640))
- FreeBSD: Limit `mcontext_t::mc_tlsbase` to FreeBSD 15 ([#&#8203;4640](https://redirect.github.com/rust-lang/libc/pull/464))
- FreeBSD: Update gating of `mcontext_t.mc_tlsbase` ([#&#8203;4703](https://redirect.github.com/rust-lang/libc/pull/4703))
- Musl s390x: Correct the definition of `statfs[64]` ([#&#8203;4549](https://redirect.github.com/rust-lang/libc/pull/4549))
- Musl s390x: Make `fpreg_t` a union ([#&#8203;4549](https://redirect.github.com/rust-lang/libc/pull/4549))
- Redox: Fix the types of `gid_t` and `uid_t` ([#&#8203;4689](https://redirect.github.com/rust-lang/libc/pull/4689))
- Redox: Fix the value of `MAP_FIXED` ([#&#8203;4684](https://redirect.github.com/rust-lang/libc/pull/4684))

##### Deprecated

- Apple: Correct the `deprecated` attribute for `iconv` ([`a97a0b53`](https://redirect.github.com/rust-lang/libc/commit/a97a0b53fb7faf5f99cd720ab12b1b8a5bf9f950))
- FreeBSD: Deprecate `TIOCMGDTRWAIT` and `TIOCMSDTRWAIT` ([#&#8203;4685](https://redirect.github.com/rust-lang/libc/pull/4685))

##### Removed

- FreeBSD: Remove `JAIL_{GET,SET}_MASK`, `_MC_FLAG_MASK` ([#&#8203;4691](https://redirect.github.com/rust-lang/libc/pull/4691))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4xNDMuMSIsInVwZGF0ZWRJblZlciI6IjQxLjE0My4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-10-13 01:07_

---

_Comment by @github-actions[bot] on 2025-10-13 01:17_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-13 01:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @charliermarsh on 2025-10-13 01:43_

---

_Closed by @charliermarsh on 2025-10-13 01:43_

---

_Branch deleted on 2025-10-13 01:43_

---

_Comment by @github-actions[bot] on 2025-10-13 01:47_

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
