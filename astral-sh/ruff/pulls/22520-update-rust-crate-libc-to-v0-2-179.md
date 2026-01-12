```yaml
number: 22520
title: Update Rust crate libc to v0.2.179
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/libc-0.x-lockfile
created_at: 2026-01-12T07:39:37Z
updated_at: 2026-01-12T07:48:45Z
url: https://github.com/astral-sh/ruff/pull/22520
synced_at: 2026-01-12T15:57:51Z
```

# Update Rust crate libc to v0.2.179

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change | Pending |
|---|---|---|---|---|
| [libc](https://redirect.github.com/rust-lang/libc) | workspace.dependencies | patch | `0.2.178` → `0.2.179` | `0.2.180` |

---

### Release Notes

<details>
<summary>rust-lang/libc (libc)</summary>

### [`v0.2.179`](https://redirect.github.com/rust-lang/libc/releases/tag/0.2.179)

[Compare Source](https://redirect.github.com/rust-lang/libc/compare/0.2.178...0.2.179)

With this release, we now have *unstable* support for 64-bit `time_t` on 32-bit
platforms with both Musl and Glibc. Testing is appreciated!

For now, these can be enabled by setting environment variables during build:

```text
RUST_LIBC_UNSTABLE_MUSL_V1_2_3=1
RUST_LIBC_UNSTABLE_GNU_TIME_BITS=64
```

Note that the exact configuration will change in the future. Setting the
`MUSL_V1_2_3` variable also enables some newer API unrelated to `time_t`.

##### Added

- L4Re: Add uclibc aarch64 support ([#&#8203;4479](https://redirect.github.com/rust-lang/libc/pull/4479))
- Linux, Android: Add a generic definition for `XCASE` ([#&#8203;4847](https://redirect.github.com/rust-lang/libc/pull/4847))
- Linux-like: Add `NAME_MAX` ([#&#8203;4888](https://redirect.github.com/rust-lang/libc/pull/4888))
- Linux: Add `AT_EXECVE_CHECK` ([#&#8203;4422](https://redirect.github.com/rust-lang/libc/pull/4422))
- Linux: Add the `SUN_LEN` macro ([#&#8203;4269](https://redirect.github.com/rust-lang/libc/pull/4269))
- Linux: add `getitimer` and `setitimer` ([#&#8203;4890](https://redirect.github.com/rust-lang/libc/pull/4890))
- Linux: add `pthread_tryjoin_n` and `pthread_timedjoin_np` ([#&#8203;4887](https://redirect.github.com/rust-lang/libc/pull/4887))
- Musl: Add unstable support for 64-bit `time_t` on 32-bit platforms ([#&#8203;4463](https://redirect.github.com/rust-lang/libc/pull/4463))
- NetBSD, OpenBSD: Add interface `LINK_STATE_*` definitions from `sys/net/if.h` ([#&#8203;4751](https://redirect.github.com/rust-lang/libc/pull/4751))
- QuRT: Add support for Qualcomm QuRT ([#&#8203;4845](https://redirect.github.com/rust-lang/libc/pull/4845))
- Types: Add Padding<T>::uninit() ([#&#8203;4862](https://redirect.github.com/rust-lang/libc/pull/4862))

##### Fixed

- Glibc: Link old version of `cf{g,s}et{i,o}speed` ([#&#8203;4882](https://redirect.github.com/rust-lang/libc/pull/4882))
- L4Re: Fixes for `pthread` ([#&#8203;4479](https://redirect.github.com/rust-lang/libc/pull/4479))
- L4re: Fix a wide variety of incorrect definitions ([#&#8203;4479](https://redirect.github.com/rust-lang/libc/pull/4479))
- Musl: Fix the value of `CPU_SETSIZE` on musl 1.2+ ([#&#8203;4865](https://redirect.github.com/rust-lang/libc/pull/4865))
- Musl: RISC-V: fix public padding fields in `stat/stat64` ([#&#8203;4463](https://redirect.github.com/rust-lang/libc/pull/4463))
- Musl: s390x: Fix definition of `SIGSTKSZ`/`MINSIGSTKSZ` ([#&#8203;4884](https://redirect.github.com/rust-lang/libc/pull/4884))
- NetBSD: Arm: Fix `PT_{GET,SET}FPREGS`, `_REG_TIPDR`, and `_REG_{LR,SP}` ([#&#8203;4899](https://redirect.github.com/rust-lang/libc/pull/4899))
- NetBSD: Fix `if_msghdr` alignment ([#&#8203;4902](https://redirect.github.com/rust-lang/libc/pull/4902))
- NetBSD: Fix `siginfo_t` layout on 32-bit platforms ([#&#8203;4904](https://redirect.github.com/rust-lang/libc/pull/4904))
- NetBSD: change definition of `pthread_spin_t` to allow arch redefinition. ([#&#8203;4899](https://redirect.github.com/rust-lang/libc/pull/4899))
- Newlib: Fix ambiguous glob exports and other warnings for Vita and 3DS ([#&#8203;4875](https://redirect.github.com/rust-lang/libc/pull/4875))
- QNX: Fix build error ([#&#8203;4879](https://redirect.github.com/rust-lang/libc/pull/4879))

##### Changed

- CI: Update CI images to FreeBSD 15.0-release ([#&#8203;4857](https://redirect.github.com/rust-lang/libc/pull/4857))
- L4Re: Make `pthread` struct fields private ([#&#8203;4876](https://redirect.github.com/rust-lang/libc/pull/4876))
- Linux, Fuchsia: Mark mq\_attr padding area as such ([#&#8203;4858](https://redirect.github.com/rust-lang/libc/pull/4858))
- Types: Wrap a number of private fields in the `Padding` type ([#&#8203;4862](https://redirect.github.com/rust-lang/libc/pull/4862))

##### Removed

- Build: Remove `RUST_LIBC_UNSTABLE_LINUX_TIME_BITS64` ([#&#8203;4865](https://redirect.github.com/rust-lang/libc/pull/4865))
- WASI: Remove nonexistent clocks ([#&#8203;4880](https://redirect.github.com/rust-lang/libc/pull/4880))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi43NC41IiwidXBkYXRlZEluVmVyIjoiNDIuNzQuNSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-12 07:39_

---

_Comment by @astral-sh-bot[bot] on 2026-01-12 07:42_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-12 07:42_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5170 diagnostics
+ Found 5169 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @MichaReiser on 2026-01-12 07:48_

---

_Closed by @MichaReiser on 2026-01-12 07:48_

---

_Branch deleted on 2026-01-12 07:48_

---
