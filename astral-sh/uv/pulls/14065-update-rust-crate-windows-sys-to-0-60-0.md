```yaml
number: 14065
title: Update Rust crate windows-sys to 0.60.0
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/windows-sys-0.x
created_at: 2025-06-16T02:28:55Z
updated_at: 2025-06-16T02:40:25Z
url: https://github.com/astral-sh/uv/pull/14065
synced_at: 2026-01-10T11:10:42Z
```

# Update Rust crate windows-sys to 0.60.0

---

_Pull request opened by @renovate on 2025-06-16 02:28_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [windows-sys](https://redirect.github.com/microsoft/windows-rs) | workspace.dependencies | minor | `0.59.0` -> `0.60.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>microsoft/windows-rs (windows-sys)</summary>

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

_Label `internal` added by @renovate[bot] on 2025-06-16 02:28_

---

_Comment by @renovate[bot] on 2025-06-16 02:28_

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
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --package windows-sys@0.59.0 --precise 0.60.2
    Updating crates.io index
error: failed to select a version for the requirement `windows-sys = "^0.59"`
candidate versions found which didn't match: 0.60.2
location searched: crates.io index
required by package `console v0.15.11`
    ... which satisfies dependency `console = "^0.15.11"` (locked to 0.15.11) of package `uv v0.7.13 (/tmp/renovate/repos/github/astral-sh/uv/crates/uv)`

```



---

_Closed by @charliermarsh on 2025-06-16 02:38_

---

_Comment by @renovate[bot] on 2025-06-16 02:40_

### Renovate Ignore Notification

Because you closed this PR without merging, Renovate will ignore this update (`0.60.0`). You will get a PR once a newer version is released. To ignore this dependency forever, add it to the `ignoreDeps` array of your Renovate config.

If you accidentally closed this PR, or if you changed your mind: rename this PR to get a fresh replacement PR.

---

_Branch deleted on 2025-06-16 02:40_

---
