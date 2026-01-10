```yaml
number: 4878
title: Update Rust crate windows to 0.58.0 - autoclosed
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/windows-0.x
created_at: 2024-07-08T00:09:57Z
updated_at: 2024-07-23T08:12:59Z
url: https://github.com/astral-sh/uv/pull/4878
synced_at: 2026-01-10T13:37:23Z
```

# Update Rust crate windows to 0.58.0 - autoclosed

---

_Pull request opened by @renovate on 2024-07-08 00:09_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [windows](https://togithub.com/microsoft/windows-rs) | dependencies | minor | `0.57.0` -> `0.58.0` |

---

### Release Notes

<details>
<summary>microsoft/windows-rs (windows)</summary>

### [`v0.58.0`](https://togithub.com/microsoft/windows-rs/releases/tag/0.58.0)

[Compare Source](https://togithub.com/microsoft/windows-rs/compare/0.57.0...0.58.0)

This release includes updates to metadata for new or fixed API definitions ([#&#8203;3111](https://togithub.com/microsoft/windows-rs/issues/3111), [#&#8203;3136](https://togithub.com/microsoft/windows-rs/issues/3136)), various improvements and fixes to code generation, compliance with new Rust warnings, additional COM authoring support improvements ([#&#8203;3065](https://togithub.com/microsoft/windows-rs/issues/3065)), limited non-Windows support ([#&#8203;3135](https://togithub.com/microsoft/windows-rs/issues/3135)), and more.

It includes major updates to the following crates, mainly due to breaking changes in metadata for API definitions.

-   `riddle` 0.58.0
-   `windows` 0.58.0
-   `windows-bindgen` 0.58.0
-   `windows-core` 0.58.0
-   `windows-implement` 0.58.0
-   `windows-interface` 0.58.0
-   `windows-metadata` 0.58.0

It also includes major updates to the following utility crates.

-   `windows-result` 0.2.0
-   `windows-registry` 0.2.0

The `windows-result` crate now provides limited non-Windows support, and the `windows-registry` crate offers new lossless queries for binary and wide string values.

And it includes minor updates to the `windows-targets` crates, with the addition of several new APIs.

-   `windows-targets` 0.52.6

This release also includes the first published version of the `windows-strings` crate, moving the string types from the `windows-core` crate into a dedicated crate as a smaller dependency. It also offers an efficient `HSTRING` builder ([#&#8203;3133](https://togithub.com/microsoft/windows-rs/issues/3133)).

To clarify, the only crates that continue to support limited non-Windows builds are:

-   `windows-bindgen` and `windows-metadata` for code generation on non-Windows platforms.
-   `windows-core` and `windows-result` for COM support on non-Windows platforms.

#### What's Changed

-   Fix default `rustfmt` for repo by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3084](https://togithub.com/microsoft/windows-rs/pull/3084)
-   Simplify standalone test by calling `windows-bindgen` directly by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3086](https://togithub.com/microsoft/windows-rs/pull/3086)
-   Disable docs and tests for test crates by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3085](https://togithub.com/microsoft/windows-rs/pull/3085)
-   Simplify pointer writes in generated code by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3089](https://togithub.com/microsoft/windows-rs/pull/3089)
-   Infer return type in generated bindings by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3090](https://togithub.com/microsoft/windows-rs/pull/3090)
-   Allow `windows-result` to work on non-Windows platforms by [@&#8203;sivadeilra](https://togithub.com/sivadeilra) in [https://github.com/microsoft/windows-rs/pull/3082](https://togithub.com/microsoft/windows-rs/pull/3082)
-   Change tests/standalone so that a tool regenerates its sources by [@&#8203;sivadeilra](https://togithub.com/sivadeilra) in [https://github.com/microsoft/windows-rs/pull/3091](https://togithub.com/microsoft/windows-rs/pull/3091)
-   Bump braces from 3.0.2 to 3.0.3 in /web/features by [@&#8203;dependabot](https://togithub.com/dependabot) in [https://github.com/microsoft/windows-rs/pull/3092](https://togithub.com/microsoft/windows-rs/pull/3092)
-   Use malloc on non-Windows platforms by [@&#8203;sivadeilra](https://togithub.com/sivadeilra) in [https://github.com/microsoft/windows-rs/pull/3095](https://togithub.com/microsoft/windows-rs/pull/3095)
-   COM interface impls move from MyApp to MyApp_Impl ("outer" object) by [@&#8203;sivadeilra](https://togithub.com/sivadeilra) in [https://github.com/microsoft/windows-rs/pull/3065](https://togithub.com/microsoft/windows-rs/pull/3065)
-   Workaround for false dead code warning by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3098](https://togithub.com/microsoft/windows-rs/pull/3098)
-   The `Debug` derive macro does not need to be qualified by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3097](https://togithub.com/microsoft/windows-rs/pull/3097)
-   Harden detection of missing nested types by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3099](https://togithub.com/microsoft/windows-rs/pull/3099)
-   Use tools to generate bindings for library crates by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3102](https://togithub.com/microsoft/windows-rs/pull/3102)
-   Allow `unused` to deal with new warning about "unused" private fields in structs  by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3103](https://togithub.com/microsoft/windows-rs/pull/3103)
-   Remove `mio` dependency by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3107](https://togithub.com/microsoft/windows-rs/pull/3107)
-   Support some edge cases for the next Win32 metadata update by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3109](https://togithub.com/microsoft/windows-rs/pull/3109)
-   Simplify how extension code for `windows` crate works by [@&#8203;sivadeilra](https://togithub.com/sivadeilra) in [https://github.com/microsoft/windows-rs/pull/3110](https://togithub.com/microsoft/windows-rs/pull/3110)
-   Directly invoke bindgen instead of recursively calling `cargo run ...` by [@&#8203;sivadeilra](https://togithub.com/sivadeilra) in [https://github.com/microsoft/windows-rs/pull/3113](https://togithub.com/microsoft/windows-rs/pull/3113)
-   Update Win32 metadata by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3111](https://togithub.com/microsoft/windows-rs/pull/3111)
-   Don't compile `windows` and `windows-sys` in unit test mode by [@&#8203;sivadeilra](https://togithub.com/sivadeilra) in [https://github.com/microsoft/windows-rs/pull/3112](https://togithub.com/microsoft/windows-rs/pull/3112)
-   Remove unused dependencies by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3117](https://togithub.com/microsoft/windows-rs/pull/3117)
-   Test cross-compilation with stable gnullvm targets by [@&#8203;mati865](https://togithub.com/mati865) in [https://github.com/microsoft/windows-rs/pull/3104](https://togithub.com/microsoft/windows-rs/pull/3104)
-   Add flexible registry type and byte query support by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3120](https://togithub.com/microsoft/windows-rs/pull/3120)
-   Remove unnecessary test cfg checks by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3124](https://togithub.com/microsoft/windows-rs/pull/3124)
-   Add `windows-strings` crate by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3125](https://togithub.com/microsoft/windows-rs/pull/3125)
-   Lock down `windows-core` internals by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3129](https://togithub.com/microsoft/windows-rs/pull/3129)
-   Remove "std" writer from `windows-bindgen` by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3130](https://togithub.com/microsoft/windows-rs/pull/3130)
-   Allow `Error` and `Result<()>` to be the same size as `HRESULT` by [@&#8203;sivadeilra](https://togithub.com/sivadeilra) in [https://github.com/microsoft/windows-rs/pull/3126](https://togithub.com/microsoft/windows-rs/pull/3126)
-   Remove boxing support from `windows-core` crate by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3131](https://togithub.com/microsoft/windows-rs/pull/3131)
-   Use conventional testing for `windows_slim_errors` by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3132](https://togithub.com/microsoft/windows-rs/pull/3132)
-   Clarify support for non-Windows targets by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3135](https://togithub.com/microsoft/windows-rs/pull/3135)
-   Add `HSTRING` builder and registry support  by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3133](https://togithub.com/microsoft/windows-rs/pull/3133)
-   Update Windows metadata to 10.0.26100.1 by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3136](https://togithub.com/microsoft/windows-rs/pull/3136)
-   Implement `Send` and `Sync` for `Weak<T>` by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3138](https://togithub.com/microsoft/windows-rs/pull/3138)
-   Remove `Future` implementation by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3142](https://togithub.com/microsoft/windows-rs/pull/3142)
-   Ensure that `HSTRING` builder provides initialized memory by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3141](https://togithub.com/microsoft/windows-rs/pull/3141)
-   Make new `windows-strings` crate Windows-only by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3143](https://togithub.com/microsoft/windows-rs/pull/3143)
-   Release 0.58.0 by [@&#8203;kennykerr](https://togithub.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3140](https://togithub.com/microsoft/windows-rs/pull/3140)

**Full Changelog**: https://github.com/microsoft/windows-rs/compare/0.57.0...0.58.0

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

This PR was generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy40MjEuOSIsInVwZGF0ZWRJblZlciI6IjM3LjQzOC4wIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-07-08 00:09_

---

_Renamed from "Update Rust crate windows to 0.58.0" to "Update Rust crate windows to 0.58.0 - autoclosed" by @renovate[bot] on 2024-07-23 08:12_

---

_Closed by @renovate[bot] on 2024-07-23 08:12_

---

_Branch deleted on 2024-07-23 08:12_

---
