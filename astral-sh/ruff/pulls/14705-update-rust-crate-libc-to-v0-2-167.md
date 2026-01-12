```yaml
number: 14705
title: Update Rust crate libc to v0.2.167
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/libc-0.x-lockfile
created_at: 2024-12-02T00:47:11Z
updated_at: 2024-12-02T01:03:38Z
url: https://github.com/astral-sh/ruff/pull/14705
synced_at: 2026-01-12T15:55:48Z
```

# Update Rust crate libc to v0.2.167

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [libc](https://redirect.github.com/rust-lang/libc) | workspace.dependencies | patch | `0.2.164` -> `0.2.167` |

---

### Release Notes

<details>
<summary>rust-lang/libc (libc)</summary>

### [`v0.2.167`](https://redirect.github.com/rust-lang/libc/releases/tag/0.2.167)

[Compare Source](https://redirect.github.com/rust-lang/libc/compare/0.2.166...0.2.167)

##### Added

-   Solarish: add `st_fstype` to `stat` [#&#8203;4145](https://redirect.github.com/rust-lang/libc/pull/4145)
-   Trusty: Add `intptr_t` and `uintptr_t` ([#&#8203;4161](https://redirect.github.com/rust-lang/libc/pull/4161))

##### Fixed

-   Fix the build with `rustc-dep-of-std` [#&#8203;4158](https://redirect.github.com/rust-lang/libc/pull/4158)
-   Wasi: Add back unsafe block for `clockid_t` static variables ([#&#8203;4157](https://redirect.github.com/rust-lang/libc/pull/4157))

##### Cleanup

-   Create an internal prelude [#&#8203;4161](https://redirect.github.com/rust-lang/libc/pull/4161)
-   Fix `unused_qualifications`[#&#8203;4132](https://redirect.github.com/rust-lang/libc/pull/4132)

##### Other

-   CI: Check various FreeBSD versions ([#&#8203;4159](https://redirect.github.com/rust-lang/libc/pull/4159))
-   CI: add a timeout for all jobs [#&#8203;4164](https://redirect.github.com/rust-lang/libc/pull/4164)
-   CI: verify MSRV for `wasm32-wasi` [#&#8203;4157](https://redirect.github.com/rust-lang/libc/pull/4157)
-   Migrate to the 2021 edition [#&#8203;4132](https://redirect.github.com/rust-lang/libc/pull/4132)

##### Removed

-   Remove one unused import after the edition 2021 bump

### [`v0.2.166`](https://redirect.github.com/rust-lang/libc/releases/tag/0.2.166)

[Compare Source](https://redirect.github.com/rust-lang/libc/compare/0.2.165...0.2.166)

##### Fixed

This release resolves two cases of unintentional breakage from the previous release:

-   Revert removal of array size hacks [#&#8203;4150](https://redirect.github.com/rust-lang/libc/pull/4150)
-   Ensure `const extern` functions are always enabled [#&#8203;4151](https://redirect.github.com/rust-lang/libc/pull/4151)

### [`v0.2.165`](https://redirect.github.com/rust-lang/libc/releases/tag/0.2.165)

[Compare Source](https://redirect.github.com/rust-lang/libc/compare/0.2.164...0.2.165)

##### Added

-   Android: add `mkostemp`, `mkostemps` [#&#8203;3601](https://redirect.github.com/rust-lang/libc/pull/3601)
-   Android: add a few API 30 calls [#&#8203;3604](https://redirect.github.com/rust-lang/libc/pull/3604)
-   Android: add missing syscall constants [#&#8203;3558](https://redirect.github.com/rust-lang/libc/pull/3558)
-   Apple: add `in6_ifreq` [#&#8203;3617](https://redirect.github.com/rust-lang/libc/pull/3617)
-   Apple: add missing `sysctl` net types [#&#8203;4022](https://redirect.github.com/rust-lang/libc/pull/4022) (before release: remove `if_family_id` ([#&#8203;4137](https://redirect.github.com/rust-lang/libc/pull/4137)))
-   Freebsd: add `kcmp` call support [#&#8203;3746](https://redirect.github.com/rust-lang/libc/pull/3746)
-   Hurd: add `MAP_32BIT` and `MAP_EXCL` [#&#8203;4127](https://redirect.github.com/rust-lang/libc/pull/4127)
-   Hurd: add `domainname` field to `utsname` ([#&#8203;4089](https://redirect.github.com/rust-lang/libc/pull/4089))
-   Linux GNU: add `f_flags` to struct `statfs` for arm, mips, powerpc and x86 [#&#8203;3663](https://redirect.github.com/rust-lang/libc/pull/3663)
-   Linux GNU: add `malloc_stats` [#&#8203;3596](https://redirect.github.com/rust-lang/libc/pull/3596)
-   Linux: add ELF relocation-related structs [#&#8203;3583](https://redirect.github.com/rust-lang/libc/pull/3583)
-   Linux: add `ptp_*` structs [#&#8203;4113](https://redirect.github.com/rust-lang/libc/pull/4113)
-   Linux: add `ptp_clock_caps` [#&#8203;4128](https://redirect.github.com/rust-lang/libc/pull/4128)
-   Linux: add `ptp_pin_function` and most `PTP_` constants [#&#8203;4114](https://redirect.github.com/rust-lang/libc/pull/4114)
-   Linux: add missing AF_XDP structs & constants [#&#8203;3956](https://redirect.github.com/rust-lang/libc/pull/3956)
-   Linux: add missing netfilter consts ([#&#8203;3734](https://redirect.github.com/rust-lang/libc/pull/3734))
-   Linux: add struct and constants for the `mount_setattr` syscall [#&#8203;4046](https://redirect.github.com/rust-lang/libc/pull/4046)
-   Linux: add wireless API [#&#8203;3441](https://redirect.github.com/rust-lang/libc/pull/3441)
-   Linux: expose the `len8_dlc` field of `can_frame` [#&#8203;3357](https://redirect.github.com/rust-lang/libc/pull/3357)
-   Musl: add `utmpx` API [#&#8203;3213](https://redirect.github.com/rust-lang/libc/pull/3213)
-   Musl: add missing syscall constants [#&#8203;4028](https://redirect.github.com/rust-lang/libc/pull/4028)
-   NetBSD: add `mcontext`-related data for RISCV64 [#&#8203;3468](https://redirect.github.com/rust-lang/libc/pull/3468)
-   Redox: add new `netinet` constants [#&#8203;3586](https://redirect.github.com/rust-lang/libc/pull/3586))
-   Solarish: add `_POSIX_VDISABLE` ([#&#8203;4103](https://redirect.github.com/rust-lang/libc/pull/4103))
-   Tests: Add a test that the `const extern fn` macro works [#&#8203;4134](https://redirect.github.com/rust-lang/libc/pull/4134)
-   Tests: Add test of primitive types against `std` [#&#8203;3616](https://redirect.github.com/rust-lang/libc/pull/3616)
-   Unix: Add `htonl`, `htons`, `ntohl`, `ntohs` [#&#8203;3669](https://redirect.github.com/rust-lang/libc/pull/3669)
-   Unix: add `aligned_alloc` [#&#8203;3843](https://redirect.github.com/rust-lang/libc/pull/3843)
-   Windows: add `aligned_realloc` [#&#8203;3592](https://redirect.github.com/rust-lang/libc/pull/3592)

##### Fixed

-   **breaking** Hurd: fix `MAP_HASSEMAPHORE` name ([#&#8203;4127](https://redirect.github.com/rust-lang/libc/pull/4127))
-   **breaking** ulibc Mips: fix `SA_*` mismatched types ([#&#8203;3211](https://redirect.github.com/rust-lang/libc/pull/3211))
-   Aix: fix an enum FFI safety warning [#&#8203;3644](https://redirect.github.com/rust-lang/libc/pull/3644)
-   Haiku: fix some typos ([#&#8203;3664](https://redirect.github.com/rust-lang/libc/pull/3664))
-   Tests: fix `Elf{32,64}_Relr`-related tests [#&#8203;3647](https://redirect.github.com/rust-lang/libc/pull/3647)
-   Tests: fix libc-tests for `loongarch64-linux-musl`
-   Tests: fix some clippy warnings [#&#8203;3855](https://redirect.github.com/rust-lang/libc/pull/3855)
-   Tests: fix tests on `riscv64gc-unknown-freebsd` [#&#8203;4129](https://redirect.github.com/rust-lang/libc/pull/4129)

##### Deprecated

-   Apple: deprecate `iconv_open` [`25e022a`](https://redirect.github.com/rust-lang/libc/commit/25e022a22eca3634166ef472b748c297e60fcf7f)
-   Apple: deprecate `mach_task_self` [#&#8203;4095](https://redirect.github.com/rust-lang/libc/pull/4095)
-   Apple: update `mach` deprecation notices for things that were removed in `main` [#&#8203;4097](https://redirect.github.com/rust-lang/libc/pull/4097)

##### Cleanup

-   Adjust the `f!` macro to be more flexible [#&#8203;4107](https://redirect.github.com/rust-lang/libc/pull/4107)
-   Aix: remove duplicate constants [#&#8203;3643](https://redirect.github.com/rust-lang/libc/pull/3643)
-   CI: make scripts more uniform [#&#8203;4042](https://redirect.github.com/rust-lang/libc/pull/4042)
-   Drop the `libc_align` conditional [`b5b553d`](https://redirect.github.com/rust-lang/libc/commit/b5b553d0ee7de0d4781432a9a9a0a6445dd7f34f)
-   Drop the `libc_cfg_target_vendor` conditional [#&#8203;4060](https://redirect.github.com/rust-lang/libc/pull/4060)
-   Drop the `libc_const_size_of` conditional [`5a43dd2`](https://redirect.github.com/rust-lang/libc/commit/5a43dd2754366f99b3a83881b30246ce0e51833c)
-   Drop the `libc_core_cvoid` conditional [#&#8203;4060](https://redirect.github.com/rust-lang/libc/pull/4060)
-   Drop the `libc_int128` conditional [#&#8203;4060](https://redirect.github.com/rust-lang/libc/pull/4060)
-   Drop the `libc_non_exhaustive` conditional [#&#8203;4060](https://redirect.github.com/rust-lang/libc/pull/4060)
-   Drop the `libc_packedN` conditional [#&#8203;4060](https://redirect.github.com/rust-lang/libc/pull/4060)
-   Drop the `libc_priv_mod_use` conditional [`19c5937`](https://redirect.github.com/rust-lang/libc/commit/19c59376d11b015009fb9b04f233a30a1bf50a91)
-   Drop the `libc_union` conditional [`b9e4d80`](https://redirect.github.com/rust-lang/libc/commit/b9e4d8012f612dfe24147da3e69522763f92b6e3)
-   Drop the `long_array` conditional [#&#8203;4096](https://redirect.github.com/rust-lang/libc/pull/4096)
-   Drop the `ptr_addr_of` conditional [#&#8203;4065](https://redirect.github.com/rust-lang/libc/pull/4065)
-   Drop warnings about deprecated cargo features [#&#8203;4060](https://redirect.github.com/rust-lang/libc/pull/4060)
-   Eliminate uses of `struct_formatter` [#&#8203;4074](https://redirect.github.com/rust-lang/libc/pull/4074)
-   Fix a few other array size hacks [`d63be8b`](https://redirect.github.com/rust-lang/libc/commit/d63be8b69b0736753213f5d933767866a5801ee7)
-   Glibc: remove redundant definitions ([#&#8203;3261](https://redirect.github.com/rust-lang/libc/pull/3261))
-   Musl: remove redundant definitions ([#&#8203;3261](https://redirect.github.com/rust-lang/libc/pull/3261))
-   Musl: unify definitions of `siginfo_t` ([#&#8203;3261](https://redirect.github.com/rust-lang/libc/pull/3261))
-   Musl: unify definitions of statfs and statfs64 ([#&#8203;3261](https://redirect.github.com/rust-lang/libc/pull/3261))
-   Musl: unify definitions of statvfs and statvfs64 ([#&#8203;3261](https://redirect.github.com/rust-lang/libc/pull/3261))
-   Musl: unify statx definitions ([#&#8203;3978](https://redirect.github.com/rust-lang/libc/pull/3978))
-   Remove array size hacks for Rust < 1.47 [`27ee6fe`](https://redirect.github.com/rust-lang/libc/commit/27ee6fe02ca0848b2af3cd747536264e4c7b697d)
-   Remove repetitive words [`77de375`](https://redirect.github.com/rust-lang/libc/commit/77de375891285e18a81616f7dceda6d52732eed6)
-   Use #\[derive] for Copy/Clone in s! and friends [#&#8203;4038](https://redirect.github.com/rust-lang/libc/pull/4038)
-   Use some tricks to format macro bodies [#&#8203;4107](https://redirect.github.com/rust-lang/libc/pull/4107)

##### Other

-   Apply formatting to macro bodies [#&#8203;4107](https://redirect.github.com/rust-lang/libc/pull/4107)
-   Bump libc-test to Rust 2021 Edition [#&#8203;3905](https://redirect.github.com/rust-lang/libc/pull/3905)
-   CI: Add a check that semver files don't contain duplicate entries [#&#8203;4087](https://redirect.github.com/rust-lang/libc/pull/4087)
-   CI: Add `fanotify_event_info_fid` to FAM-exempt types [#&#8203;4038](https://redirect.github.com/rust-lang/libc/pull/4038)
-   CI: Allow rustfmt to organize imports ([#&#8203;4136](https://redirect.github.com/rust-lang/libc/pull/4136))
-   CI: Always run rustfmt [#&#8203;4120](https://redirect.github.com/rust-lang/libc/pull/4120)
-   CI: Change 32-bit Docker images to use EOL repos [#&#8203;4120](https://redirect.github.com/rust-lang/libc/pull/4120)
-   CI: Change 64-bit Docker images to ubuntu:24.10 [#&#8203;4120](https://redirect.github.com/rust-lang/libc/pull/4120)
-   CI: Disable the check for >1 s! invocation [#&#8203;4107](https://redirect.github.com/rust-lang/libc/pull/4107)
-   CI: Ensure build channels get run even if FILTER is unset [#&#8203;4125](https://redirect.github.com/rust-lang/libc/pull/4125)
-   CI: Ensure there is a fallback for no_std [#&#8203;4125](https://redirect.github.com/rust-lang/libc/pull/4125)
-   CI: Fix cases where unset variables cause errors [#&#8203;4108](https://redirect.github.com/rust-lang/libc/pull/4108)
-   CI: Naming adjustments and cleanup [#&#8203;4124](https://redirect.github.com/rust-lang/libc/pull/4124)
-   CI: Only invoke rustup if running in CI [#&#8203;4107](https://redirect.github.com/rust-lang/libc/pull/4107)
-   CI: Remove the logic to handle old rust versions [#&#8203;4068](https://redirect.github.com/rust-lang/libc/pull/4068)
-   CI: Set -u (error on unset) in all script files [#&#8203;4108](https://redirect.github.com/rust-lang/libc/pull/4108)
-   CI: add support for `loongarch64-unknown-linux-musl` [#&#8203;4092](https://redirect.github.com/rust-lang/libc/pull/4092)
-   CI: make `aarch64-apple-darwin` not a nightly-only target [#&#8203;4068](https://redirect.github.com/rust-lang/libc/pull/4068)
-   CI: run shellcheck on all scripts [#&#8203;4042](https://redirect.github.com/rust-lang/libc/pull/4042)
-   CI: update musl headers to Linux 6.6 [#&#8203;3921](https://redirect.github.com/rust-lang/libc/pull/3921)
-   CI: use qemu-sparc64 to run sparc64 tests [#&#8203;4133](https://redirect.github.com/rust-lang/libc/pull/4133)
-   Drop the `libc_const_extern_fn` conditional [`674cc1f`](https://redirect.github.com/rust-lang/libc/commit/674cc1f47f605038ef1aa2cce8e8bc9dac128276)
-   Drop the `libc_underscore_const_names` conditional [`f0febd5`](https://redirect.github.com/rust-lang/libc/commit/f0febd5e2e50b38e05259d3afad3c9783711bcf0)
-   Explicitly set the edition to 2015 [#&#8203;4058](https://redirect.github.com/rust-lang/libc/pull/4058)
-   Introduce a `git-blame-ignore-revs` file [#&#8203;4107](https://redirect.github.com/rust-lang/libc/pull/4107)
-   Tests: Ignore fields as required on Ubuntu 24.10 [#&#8203;4120](https://redirect.github.com/rust-lang/libc/pull/4120)
-   Tests: skip `ATF_*` constants for OpenBSD [#&#8203;4088](https://redirect.github.com/rust-lang/libc/pull/4088)
-   Triagebot: Add an autolabel for CI [#&#8203;4052](https://redirect.github.com/rust-lang/libc/pull/4052)

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xOS4wIiwidXBkYXRlZEluVmVyIjoiMzkuMTkuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-12-02 00:47_

---

_Merged by @charliermarsh on 2024-12-02 01:03_

---

_Closed by @charliermarsh on 2024-12-02 01:03_

---

_Branch deleted on 2024-12-02 01:03_

---

_Comment by @github-actions[bot] on 2024-12-02 01:03_

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
