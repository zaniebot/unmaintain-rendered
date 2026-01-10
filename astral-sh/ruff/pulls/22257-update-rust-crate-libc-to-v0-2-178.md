```yaml
number: 22257
title: Update Rust crate libc to v0.2.178
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/libc-0.x-lockfile
created_at: 2025-12-29T16:12:19Z
updated_at: 2025-12-29T17:24:56Z
url: https://github.com/astral-sh/ruff/pull/22257
synced_at: 2026-01-10T16:36:18Z
```

# Update Rust crate libc to v0.2.178

---

_Pull request opened by @renovate on 2025-12-29 16:12_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [libc](https://redirect.github.com/rust-lang/libc) | workspace.dependencies | patch | `0.2.177` -> `0.2.178` |

---

### Release Notes

<details>
<summary>rust-lang/libc (libc)</summary>

### [`v0.2.178`](https://redirect.github.com/rust-lang/libc/releases/tag/0.2.178)

[Compare Source](https://redirect.github.com/rust-lang/libc/compare/0.2.177...0.2.178)

##### Added

- BSD: Add `issetugid` ([#&#8203;4744](https://redirect.github.com/rust-lang/libc/pull/4744))
- Cygwin: Add missing utmp/x.h, grp.h, and stdio.h interfaces ([#&#8203;4827](https://redirect.github.com/rust-lang/libc/pull/4827))
- Linux s390x musl: Add `__psw_t`/`fprefset_t`/`*context_t` ([#&#8203;4726](https://redirect.github.com/rust-lang/libc/pull/4726))
- Linux, Android: Add definition for IUCLC ([#&#8203;4846](https://redirect.github.com/rust-lang/libc/pull/4846))
- Linux, FreeBSD: Add `AT_HWCAP{3,4}` ([#&#8203;4734](https://redirect.github.com/rust-lang/libc/pull/4734))
- Linux: Add definitions from linux/can/bcm.h ([#&#8203;4683](https://redirect.github.com/rust-lang/libc/pull/4683))
- Linux: Add syscalls 451-469 for m68k ([#&#8203;4850](https://redirect.github.com/rust-lang/libc/pull/4850))
- Linux: PowerPC: Add 'ucontext.h' definitions ([#&#8203;4696](https://redirect.github.com/rust-lang/libc/pull/4696))
- NetBSD: Define `eventfd` ([#&#8203;4830](https://redirect.github.com/rust-lang/libc/pull/4830))
- Newlib: Add missing constants from `unistd.h` ([#&#8203;4811](https://redirect.github.com/rust-lang/libc/pull/4811))
- QNX NTO: Add `cfmakeraw` ([#&#8203;4704](https://redirect.github.com/rust-lang/libc/pull/4704))
- QNX NTO: Add `cfsetspeed` ([#&#8203;4704](https://redirect.github.com/rust-lang/libc/pull/4704))
- Redox: Add `getresgid` and `getresuid` ([#&#8203;4752](https://redirect.github.com/rust-lang/libc/pull/4752))
- Redox: Add `setresgid` and `setresuid` ([#&#8203;4752](https://redirect.github.com/rust-lang/libc/pull/4752))
- VxWorks: Add definitions from `select.h`, `stat.h`, `poll.h`, `ttycom.h`, `utsname.h`, `resource.h`, `mman.h`, `udp.h`, `in.h`, `in6.h`, `if.h`, `fnmatch.h`, and `sioLibCommon.h` ([#&#8203;4781](https://redirect.github.com/rust-lang/libc/pull/4781))
- VxWorks: Add missing defines/functions needed by rust stdlib ([#&#8203;4779](https://redirect.github.com/rust-lang/libc/pull/4779))
- WASI: Add more definitions for libstd ([#&#8203;4747](https://redirect.github.com/rust-lang/libc/pull/4747))

##### Deprecated:

- Apple: Deprecate `TIOCREMOTE` ([#&#8203;4764](https://redirect.github.com/rust-lang/libc/pull/4764))

##### Fixed:

Note that there were a large number of fixes on NetBSD for this `libc` release, some of which include minor breakage.

- AIX: Change errno `EWOULDBLOCK` to make it an alias of `EAGAIN` ([#&#8203;4790](https://redirect.github.com/rust-lang/libc/pull/4790))
- AIX: Resolve function comparison and `unnecessary_transmutes` warnings ([#&#8203;4780](https://redirect.github.com/rust-lang/libc/pull/4780))
- Apple: Correct the value of `SF_SETTABLE` ([#&#8203;4764](https://redirect.github.com/rust-lang/libc/pull/4764))
- DragonflyBSD: Fix the type of `mcontext_t.mc_fpregs` ([#]())
- EspIDF: Fix the duplicate definition of `gethostname` ([#&#8203;4773](https://redirect.github.com/rust-lang/libc/pull/4773))
- L4Re: Update available pthread API ([#&#8203;4836](https://redirect.github.com/rust-lang/libc/pull/4836))
- Linux: Correct the value of `NFT_MSG_MAX` ([#&#8203;4761](https://redirect.github.com/rust-lang/libc/pull/4761))
- Linux: Remove incorrect `repr(align(8))` for `canxl_frame` ([#&#8203;4760](https://redirect.github.com/rust-lang/libc/pull/4760))
- Make `eventfd` argument names match OS docs/headers ([#&#8203;4830](https://redirect.github.com/rust-lang/libc/pull/4830))
- NetBSD: Account for upstream changes to ptrace with LWP ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Correct `ipc_perm`, split from OpenBSD as `ipc.rs` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Correct a number of symbol link names ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Correct the type of `kinfo_vmentry.kve_path` ([#]())
- NetBSD: Fix `uucred.cr_ngroups` from `int` to `short` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Fix the type of `kevent.udata` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Fix the type of `mcontext_t.__fpregs` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Fix the value of `PT_SUSPEND` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Fix the values of FNM\_\* constants ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Increase the size of `sockaddr_dl.sdl_data` from 12 to 24 ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Introduce `if_.rs`, fix the definition of `ifreq` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Introduce `time.rs`, fix the values of `CLOCK_*_CPUTIME_ID` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Introduce `timex.rs` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Introduce `types.rs`, correct the definition of `lwpid_t` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Introduce `utmp_.rs`, correct the definition of `lastlog` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Introduce `utmpx_.rs`, correct utmpx definitions ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Make `_cpuset` an extern type ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: RISC-V 64: Fix the `mcontext` types ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- Nuttx: Resolve warnings ([#&#8203;4773](https://redirect.github.com/rust-lang/libc/pull/4773))
- OHOS: Don't emit duplicate lfs64 definitions ([#&#8203;4804](https://redirect.github.com/rust-lang/libc/pull/4804))
- Redox: Fix the type of `pid_t` ([#&#8203;4825](https://redirect.github.com/rust-lang/libc/pull/4825))
- WASI: Gate `__wasilibc_register_preopened_fd`  ([#&#8203;4837](https://redirect.github.com/rust-lang/libc/pull/4837))
- Wali: Fix unknown config ([#&#8203;4773](https://redirect.github.com/rust-lang/libc/pull/4773))

##### Changed

- AIX: Declare field 'tv\_nsec' of structure 'timespec' as 'i32' in both 32-bit and 64-bit modes ([#&#8203;4750](https://redirect.github.com/rust-lang/libc/pull/4750))
- DragonFly: Avoid usage of `thread_local` ([#&#8203;3653](https://redirect.github.com/rust-lang/libc/pull/3653))
- Linux: Update the definition for `ucontext_t` and unskip its tests ([#&#8203;4760](https://redirect.github.com/rust-lang/libc/pull/4760))
- MinGW: Set `L_tmpnam` and `TMP_MAX` to the UCRT value ([#&#8203;4566](https://redirect.github.com/rust-lang/libc/pull/4566))
- WASI: More closely align pthread type reprs ([#&#8203;4747](https://redirect.github.com/rust-lang/libc/pull/4747))
- Simplify rustc-check-cfg emission in build.rs ([#&#8203;4724](https://redirect.github.com/rust-lang/libc/pull/4724))
- Transition a number of definitions to the new source structure (internal change)

##### Removed

- MIPS Musl: Remove rogue definition of `SIGSTKFLT` ([#&#8203;4749](https://redirect.github.com/rust-lang/libc/pull/4749))
- NetBSD: Make `statvfs.f_spare` non-public ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Remove BPF constants ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Remove `*_MAXID` constants and `AT_SUN_LDPGSIZE` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Remove `IFF_NOTRAILERS` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Remove `vm_size_t` ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))
- NetBSD: Replace REG\_ENOSYS with REG\_ILLSEQ ([#&#8203;4782](https://redirect.github.com/rust-lang/libc/pull/4782))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi41OS4wIiwidXBkYXRlZEluVmVyIjoiNDIuNTkuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-12-29 16:12_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 16:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-29 16:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`


```

</details>


No memory usage changes detected ✅



---

_@woodruffw approved on 2025-12-29 16:41_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 17:24_


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

_Merged by @MichaReiser on 2025-12-29 17:24_

---

_Closed by @MichaReiser on 2025-12-29 17:24_

---

_Branch deleted on 2025-12-29 17:24_

---
