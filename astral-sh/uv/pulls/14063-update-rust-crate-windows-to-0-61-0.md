```yaml
number: 14063
title: Update Rust crate windows to 0.61.0
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/windows-0.x
created_at: 2025-06-16T02:08:23Z
updated_at: 2025-06-16T09:06:05Z
url: https://github.com/astral-sh/uv/pull/14063
synced_at: 2026-01-12T16:11:00Z
```

# Update Rust crate windows to 0.61.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [windows](https://redirect.github.com/microsoft/windows-rs) | workspace.dependencies | minor | `0.59.0` -> `0.61.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>microsoft/windows-rs (windows)</summary>

### [`v0.61.0`](https://redirect.github.com/microsoft/windows-rs/releases/tag/0.61.0): 61

[Compare Source](https://redirect.github.com/microsoft/windows-rs/compare/0.60.0...0.61.0)

Major crate updates:

-   `windows` 0.59.0
-   `windows-core` 0.59.0
    -   `windows-implement` 0.59.0
    -   `windows-interface` 0.59.0
-   `windows-targets` 0.53.0
    -   `windows_i686_msvc` 0.53.0
    -   `windows_x86_64_msvc` 0.53.0
    -   `windows_aarch64_msvc` 0.53.0
    -   `windows_i686_gnu` 0.53.0
    -   `windows_x86_64_gnu` 0.53.0
    -   `windows_i686_gnullvm` 0.53.0
    -   `windows_x86_64_gnullvm` 0.53.0
    -   `windows_aarch64_gnullvm` 0.53.0
-   `windows-bindgen` 0.59.0
-   `windows-registry` 0.4.0
-   `windows-result` 0.3.0
-   `windows-strings` 0.3.0
-   `cppwinrt` 0.2.0

Minor crate updates:

-   `windows-version` 0.1.2

Excluded:

-   `windows-sys` 0.59.0

Things to keep in mind:

-   The tag/release names no longer map directly to the crate versions, so to [find samples](https://redirect.github.com/microsoft/windows-rs/tree/master/crates/samples) for a particular release requires looking at [the releases](https://redirect.github.com/microsoft/windows-rs/releases) page and finding the release that most recently updated a particular crate.

-   The `windows-bindgen` crate includes the major code generation overhaul that brings many improvements - be sure to check out the PR description for more information. The resulting code gen depends on the new version of `windows-core` and its dependencies, unless you include the `--sys` option. [#&#8203;3359](https://redirect.github.com/microsoft/windows-rs/issues/3359)

-   The `cppwinrt` crate constitutes a major update due to streamlining the error handling. [#&#8203;3415](https://redirect.github.com/microsoft/windows-rs/issues/3415)

-   The `windows-registry`, `windows-strings,` and `windows-result` crates are also major version updates since they include small breaking changes.

-   The `windows-targets` crate finally receives a major version update, the first in over a year. This is due to [#&#8203;3359](https://redirect.github.com/microsoft/windows-rs/issues/3359) and [#&#8203;3342](https://redirect.github.com/microsoft/windows-rs/issues/3342) potentially introducing breaking changes. Although unlikely, these updates introduced sufficient changes that make it hard to ensure that the `windows-targets` libs don't break existing code. As we're updating `windows-targets` anyway, I took the liberty to bump the MSRV to 1.60 - to match the latest version of `windows-sys` - and remove the old but unused doc macro feature. Both remained for compatibility with very old dependents of the `windows-targets` crate.

-   The `windows-version` crate receives a minor update to update its dependency on the `windows-targets` crate.

-   Beyond these specifics, this update is the culmination of around 6 months worth of work on the `windows-rs` project. The biggest improvements comes from the new code generation engine, but many other improvements are now also available for production. This includes support for many new lints, warnings, and suggestions provided by the Rust toolchain; much smaller code gen thanks to deriving many more traits; more efficient code gen; major improvements to WinRT type system and implementation support; more robust and consistent error handling; stock collection and async support; improved support for class hierarchies; and much more!

In addition to "what's changed" below, check out what's changed for notes for [0.60.0](https://redirect.github.com/microsoft/windows-rs/releases/tag/0.60.0) and [0.59.0](https://redirect.github.com/microsoft/windows-rs/releases/tag/0.59.0) for additional changes that roll up to the crates published as part of this release.

#### What's Changed

-   Remove improper_ctypes workaround by [@&#8203;ChrisDenton](https://redirect.github.com/ChrisDenton) in [https://github.com/microsoft/windows-rs/pull/3296](https://redirect.github.com/microsoft/windows-rs/pull/3296)
-   Bump rollup from 2.79.1 to 2.79.2 in /web/features by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/microsoft/windows-rs/pull/3299](https://redirect.github.com/microsoft/windows-rs/pull/3299)
-   Update jsonschema requirement from 0.20 to 0.21 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/microsoft/windows-rs/pull/3301](https://redirect.github.com/microsoft/windows-rs/pull/3301)
-   Address Rust nightly compiler warnings by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3311](https://redirect.github.com/microsoft/windows-rs/pull/3311)
-   Update jsonschema requirement from 0.21 to 0.22 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/microsoft/windows-rs/pull/3310](https://redirect.github.com/microsoft/windows-rs/pull/3310)
-   Update workflows to ignore paths on pull request by [@&#8203;riverar](https://redirect.github.com/riverar) in [https://github.com/microsoft/windows-rs/pull/3312](https://redirect.github.com/microsoft/windows-rs/pull/3312)
-   Fix remaining `std` references in `windows` and `windows-core` crates for `no_std` builds by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3317](https://redirect.github.com/microsoft/windows-rs/pull/3317)
-   Bump cookie and express in /web/features by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/microsoft/windows-rs/pull/3318](https://redirect.github.com/microsoft/windows-rs/pull/3318)
-   Fix nested struct sort order  by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3321](https://redirect.github.com/microsoft/windows-rs/pull/3321)
-   Update jsonschema requirement from 0.22 to 0.23 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/microsoft/windows-rs/pull/3323](https://redirect.github.com/microsoft/windows-rs/pull/3323)
-   Add `unwrap` helper for `NTSTATUS` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3324](https://redirect.github.com/microsoft/windows-rs/pull/3324)
-   Bump http-proxy-middleware from 2.0.6 to 2.0.7 in /web/features by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/microsoft/windows-rs/pull/3331](https://redirect.github.com/microsoft/windows-rs/pull/3331)
-   Remove "implement" feature  by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3333](https://redirect.github.com/microsoft/windows-rs/pull/3333)
-   Update web workflow by [@&#8203;riverar](https://redirect.github.com/riverar) in [https://github.com/microsoft/windows-rs/pull/3344](https://redirect.github.com/microsoft/windows-rs/pull/3344)
-   Update Windows metadata by [@&#8203;riverar](https://redirect.github.com/riverar) in [https://github.com/microsoft/windows-rs/pull/3342](https://redirect.github.com/microsoft/windows-rs/pull/3342)
-   Bump cross-spawn from 7.0.3 to 7.0.6 in /web/features by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/microsoft/windows-rs/pull/3347](https://redirect.github.com/microsoft/windows-rs/pull/3347)
-   fix: remove use of std in windows-strings h! macro by [@&#8203;vthib](https://redirect.github.com/vthib) in [https://github.com/microsoft/windows-rs/pull/3356](https://redirect.github.com/microsoft/windows-rs/pull/3356)
-   Major `windows-bindgen` update by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3359](https://redirect.github.com/microsoft/windows-rs/pull/3359)
-   Harden reg-free class activation by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3365](https://redirect.github.com/microsoft/windows-rs/pull/3365)
-   Simpler bindings generation by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3367](https://redirect.github.com/microsoft/windows-rs/pull/3367)
-   `windows-bindgen` should generate `no_std` bindings by default by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3366](https://redirect.github.com/microsoft/windows-rs/pull/3366)
-   Prefer optional over convertible parameters by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3368](https://redirect.github.com/microsoft/windows-rs/pull/3368)
-   Remove unused extensions by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3369](https://redirect.github.com/microsoft/windows-rs/pull/3369)
-   Simpler code generation by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3370](https://redirect.github.com/microsoft/windows-rs/pull/3370)
-   Update dependencies by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3374](https://redirect.github.com/microsoft/windows-rs/pull/3374)
-   Simpler code gen for Boolean parameters by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3373](https://redirect.github.com/microsoft/windows-rs/pull/3373)
-   Remap `BOOLEAN` to `bool` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3376](https://redirect.github.com/microsoft/windows-rs/pull/3376)
-   Avoid generating `transmute` for input value type parameter bindings by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3377](https://redirect.github.com/microsoft/windows-rs/pull/3377)
-   Fix macro docs by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3378](https://redirect.github.com/microsoft/windows-rs/pull/3378)
-   Streamline error handling in `windows-bindgen` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3379](https://redirect.github.com/microsoft/windows-rs/pull/3379)
-   Improve WinRT event representation and testing by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3382](https://redirect.github.com/microsoft/windows-rs/pull/3382)
-   Use `track_caller` to make debugging `bindgen` build script errors easier by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3383](https://redirect.github.com/microsoft/windows-rs/pull/3383)
-   `windows-bindgen` now uses `Ref` and `OutRef` for COM interface traits by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3386](https://redirect.github.com/microsoft/windows-rs/pull/3386)
-   Add static event test/sample for WinRT by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3389](https://redirect.github.com/microsoft/windows-rs/pull/3389)
-   Verify param direction by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3390](https://redirect.github.com/microsoft/windows-rs/pull/3390)
-   Ensure external references to static factories are generated correctly by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3392](https://redirect.github.com/microsoft/windows-rs/pull/3392)
-   Update `windows-bindgen` to support `unsafe_op_in_unsafe_fn` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3393](https://redirect.github.com/microsoft/windows-rs/pull/3393)
-   Make `Ref` work with more than just interface types by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3394](https://redirect.github.com/microsoft/windows-rs/pull/3394)
-   Add PR preview deployments to web workflow by [@&#8203;riverar](https://redirect.github.com/riverar) in [https://github.com/microsoft/windows-rs/pull/3395](https://redirect.github.com/microsoft/windows-rs/pull/3395)
-   Adjust web workflow to use gh-pages branch by [@&#8203;riverar](https://redirect.github.com/riverar) in [https://github.com/microsoft/windows-rs/pull/3397](https://redirect.github.com/microsoft/windows-rs/pull/3397)
-   Streamline CRA deps and webpack config by [@&#8203;riverar](https://redirect.github.com/riverar) in [https://github.com/microsoft/windows-rs/pull/3396](https://redirect.github.com/microsoft/windows-rs/pull/3396)
-   Address nightly clippy warnings about operator precedence by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3414](https://redirect.github.com/microsoft/windows-rs/pull/3414)
-   Detect unsupported array parameters by [@&#8203;iancormac84](https://redirect.github.com/iancormac84) in [https://github.com/microsoft/windows-rs/pull/3402](https://redirect.github.com/microsoft/windows-rs/pull/3402)
-   `cppwinrt` should consistently panic on failure  by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3415](https://redirect.github.com/microsoft/windows-rs/pull/3415)
-   Shorten sample crate names by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3416](https://redirect.github.com/microsoft/windows-rs/pull/3416)
-   Use `track_caller` to make debugging `cppwinrt` build script errors easier by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3417](https://redirect.github.com/microsoft/windows-rs/pull/3417)
-   Fix provenance in direct32 sample by [@&#8203;ChrisDenton](https://redirect.github.com/ChrisDenton) in [https://github.com/microsoft/windows-rs/pull/3419](https://redirect.github.com/microsoft/windows-rs/pull/3419)
-   Update web workflow to use external origin by [@&#8203;riverar](https://redirect.github.com/riverar) in [https://github.com/microsoft/windows-rs/pull/3420](https://redirect.github.com/microsoft/windows-rs/pull/3420)
-   Avoid `transmute` where possible by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3421](https://redirect.github.com/microsoft/windows-rs/pull/3421)
-   Update GitHub Actions runners by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3423](https://redirect.github.com/microsoft/windows-rs/pull/3423)
-   Improve feature search UX, add dark mode, and update deps by [@&#8203;riverar](https://redirect.github.com/riverar) in [https://github.com/microsoft/windows-rs/pull/3422](https://redirect.github.com/microsoft/windows-rs/pull/3422)
-   Release 0.61.0 by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3418](https://redirect.github.com/microsoft/windows-rs/pull/3418)

#### New Contributors

-   [@&#8203;iancormac84](https://redirect.github.com/iancormac84) made their first contribution in [https://github.com/microsoft/windows-rs/pull/3402](https://redirect.github.com/microsoft/windows-rs/pull/3402)

**Full Changelog**: https://github.com/microsoft/windows-rs/compare/0.60.0...0.61.0

### [`v0.60.0`](https://redirect.github.com/microsoft/windows-rs/releases/tag/0.60.0)

[Compare Source](https://redirect.github.com/microsoft/windows-rs/compare/0.59.0...0.60.0)

This release includes an update to the [windows-registry](https://crates.io/crates/windows-registry) and [windows-strings](https://crates.io/crates/windows-strings) crates, mainly to provide various improvements to registry support for [rustup](https://redirect.github.com/rust-lang/rustup).

#### What's Changed

-   Add precise registry types and allocation-free queries and updates by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3184](https://redirect.github.com/microsoft/windows-rs/pull/3184)
-   Add registry `Value` to/from `HSTRING` conversion by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3190](https://redirect.github.com/microsoft/windows-rs/pull/3190)
-   Replace `From<&str>` for `GUID` with `TryFrom<&str>` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3193](https://redirect.github.com/microsoft/windows-rs/pull/3193)
-   Remove uneeded feature dependencies by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3201](https://redirect.github.com/microsoft/windows-rs/pull/3201)
-   docs: add root level documentation for all libraries by [@&#8203;Nerixyz](https://redirect.github.com/Nerixyz) in [https://github.com/microsoft/windows-rs/pull/3202](https://redirect.github.com/microsoft/windows-rs/pull/3202)
-   Cleanup doc testing by [@&#8203;Nerixyz](https://redirect.github.com/Nerixyz) in [https://github.com/microsoft/windows-rs/pull/3205](https://redirect.github.com/microsoft/windows-rs/pull/3205)
-   Revert cfg doc by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3206](https://redirect.github.com/microsoft/windows-rs/pull/3206)
-   Remove workaround for "unused" private fields by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3207](https://redirect.github.com/microsoft/windows-rs/pull/3207)
-   Immutable Event implementation by [@&#8203;lifers](https://redirect.github.com/lifers) in [https://github.com/microsoft/windows-rs/pull/3198](https://redirect.github.com/microsoft/windows-rs/pull/3198)
-   Always treat warnings as errors by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3210](https://redirect.github.com/microsoft/windows-rs/pull/3210)
-   Consistent allocation failure handling by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3209](https://redirect.github.com/microsoft/windows-rs/pull/3209)
-   Improve class hierarchy support by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3212](https://redirect.github.com/microsoft/windows-rs/pull/3212)
-   Consistent allocation failure for stock collections by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3216](https://redirect.github.com/microsoft/windows-rs/pull/3216)
-   Consistent allocation failure for `windows-registry` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3215](https://redirect.github.com/microsoft/windows-rs/pull/3215)
-   Add default "std" feature for `windows-registry` crate by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3214](https://redirect.github.com/microsoft/windows-rs/pull/3214)
-   Overhaul async and future support by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3213](https://redirect.github.com/microsoft/windows-rs/pull/3213)
-   Addressing new nightly Clippy warning by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3222](https://redirect.github.com/microsoft/windows-rs/pull/3222)
-   Add async `ready` support by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3221](https://redirect.github.com/microsoft/windows-rs/pull/3221)
-   Bump micromatch from 4.0.5 to 4.0.8 in /web/features by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/microsoft/windows-rs/pull/3223](https://redirect.github.com/microsoft/windows-rs/pull/3223)
-   Add file dialog sample by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3226](https://redirect.github.com/microsoft/windows-rs/pull/3226)
-   Use relative path for extension by [@&#8203;glandium](https://redirect.github.com/glandium) in [https://github.com/microsoft/windows-rs/pull/3224](https://redirect.github.com/microsoft/windows-rs/pull/3224)
-   Simplify trait bounds for interface implementations by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3227](https://redirect.github.com/microsoft/windows-rs/pull/3227)
-   Remove unnecessary closure from generated code by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3228](https://redirect.github.com/microsoft/windows-rs/pull/3228)
-   Bump webpack from 5.90.2 to 5.94.0 in /web/features by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/microsoft/windows-rs/pull/3236](https://redirect.github.com/microsoft/windows-rs/pull/3236)
-   Add async `spawn` support by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3235](https://redirect.github.com/microsoft/windows-rs/pull/3235)
-   Nightly Clippy warning about assumed lifetime by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3243](https://redirect.github.com/microsoft/windows-rs/pull/3243)
-   Regenerate GNU libs by [@&#8203;riverar](https://redirect.github.com/riverar) in [https://github.com/microsoft/windows-rs/pull/3241](https://redirect.github.com/microsoft/windows-rs/pull/3241)
-   Add support for composable constructors by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3246](https://redirect.github.com/microsoft/windows-rs/pull/3246)
-   Use workspace dependencies where practical by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3248](https://redirect.github.com/microsoft/windows-rs/pull/3248)
-   Add test folders by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3252](https://redirect.github.com/microsoft/windows-rs/pull/3252)
-   Improve interop testing by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3253](https://redirect.github.com/microsoft/windows-rs/pull/3253)
-   Avoid deriving `Eq` for structs containing floating point type parameters by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3255](https://redirect.github.com/microsoft/windows-rs/pull/3255)
-   Add test for composable type authoring support by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3259](https://redirect.github.com/microsoft/windows-rs/pull/3259)
-   Factory cache statics don't need to be public by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3261](https://redirect.github.com/microsoft/windows-rs/pull/3261)
-   Allow `noexcept` methods in a composable hierarchy by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3262](https://redirect.github.com/microsoft/windows-rs/pull/3262)
-   Group more of the WinRT tests together by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3263](https://redirect.github.com/microsoft/windows-rs/pull/3263)
-   Remove "riddle" and metadata generation by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3266](https://redirect.github.com/microsoft/windows-rs/pull/3266)
-   Improvements to `windows-metadata` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3268](https://redirect.github.com/microsoft/windows-rs/pull/3268)
-   We can now derive `Eq` and `PartialEq` for structs containing callbacks by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3270](https://redirect.github.com/microsoft/windows-rs/pull/3270)
-   Simpler "retval" heuristic by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3271](https://redirect.github.com/microsoft/windows-rs/pull/3271)
-   Test error handling for `windows-bindgen` crate by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3272](https://redirect.github.com/microsoft/windows-rs/pull/3272)
-   Exclude `web` on most workflows by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3279](https://redirect.github.com/microsoft/windows-rs/pull/3279)
-   Bump serve-static and express in /web/features by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/microsoft/windows-rs/pull/3274](https://redirect.github.com/microsoft/windows-rs/pull/3274)
-   Update jsonschema requirement from 0.18 to 0.19 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/microsoft/windows-rs/pull/3283](https://redirect.github.com/microsoft/windows-rs/pull/3283)
-   Move `VARIANT` support to the `windows` crate by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3282](https://redirect.github.com/microsoft/windows-rs/pull/3282)
-   Update `jsonschema` dependency by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3286](https://redirect.github.com/microsoft/windows-rs/pull/3286)
-   Expand `raw-dylib` testing by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3287](https://redirect.github.com/microsoft/windows-rs/pull/3287)
-   Fix for `cppwinrt` concurrency issue by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3289](https://redirect.github.com/microsoft/windows-rs/pull/3289)
-   Address Rust nightly compiler warnings by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3292](https://redirect.github.com/microsoft/windows-rs/pull/3292)
-   Add `Deref` implementation for `HSTRING` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3291](https://redirect.github.com/microsoft/windows-rs/pull/3291)
-   Release 0.60.0 by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [https://github.com/microsoft/windows-rs/pull/3293](https://redirect.github.com/microsoft/windows-rs/pull/3293)

#### New Contributors

-   [@&#8203;lifers](https://redirect.github.com/lifers) made their first contribution in [https://github.com/microsoft/windows-rs/pull/3198](https://redirect.github.com/microsoft/windows-rs/pull/3198)

**Full Changelog**: https://github.com/microsoft/windows-rs/compare/0.59.0...0.60.0

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC41MC4wIiwidXBkYXRlZEluVmVyIjoiNDAuNTAuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Comment by @renovate[bot] on 2025-06-16 02:08_

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
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --package windows@0.59.0 --precise 0.61.3
error: package ID specification `windows@0.59.0` did not match any packages
help: there are similar package ID specifications:

  windows@0.57.0
  windows@0.61.1

```



---

_Label `internal` added by @renovate[bot] on 2025-06-16 02:08_

---

_Comment by @konstin on 2025-06-16 09:04_

The ecosystem isn't there yet, these updates need to be coordinated

---

_Closed by @konstin on 2025-06-16 09:04_

---

_Comment by @renovate[bot] on 2025-06-16 09:06_

### Renovate Ignore Notification

Because you closed this PR without merging, Renovate will ignore this update (`0.61.0`). You will get a PR once a newer version is released. To ignore this dependency forever, add it to the `ignoreDeps` array of your Renovate config.

If you accidentally closed this PR, or if you changed your mind: rename this PR to get a fresh replacement PR.

---

_Branch deleted on 2025-06-16 09:06_

---
