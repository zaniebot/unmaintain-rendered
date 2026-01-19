```yaml
number: 17600
title: Update Rust crate windows to 0.62.0
type: pull_request
state: open
author: renovate
labels:
  - internal
  - "build:skip-release"
assignees: []
base: main
head: renovate/windows-0.x
created_at: 2026-01-19T01:35:23Z
updated_at: 2026-01-19T23:16:35Z
url: https://github.com/astral-sh/uv/pull/17600
synced_at: 2026-01-19T23:36:51Z
```

# Update Rust crate windows to 0.62.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [windows](https://redirect.github.com/microsoft/windows-rs) | dependencies | minor | `0.61.0` → `0.62.0` |
| [windows](https://redirect.github.com/microsoft/windows-rs) | workspace.dependencies | minor | `0.61.0` → `0.62.0` |

---

### Release Notes

<details>
<summary>microsoft/windows-rs (windows)</summary>

### [`v0.62.0`](https://redirect.github.com/microsoft/windows-rs/releases/tag/0.62.0): 62

##### New crates in this release

- The [windows-collections](https://crates.io/crates/windows-collections) crate defines the Windows collection types like `IIterable<T>`, `IVector<T>`, `IMap<K, V>`, and so on ([#&#8203;3483](https://redirect.github.com/microsoft/windows-rs/issues/3483)). It also includes all of the stock implementations for creating such collections ([#&#8203;2346](https://redirect.github.com/microsoft/windows-rs/issues/2346), [#&#8203;2350](https://redirect.github.com/microsoft/windows-rs/issues/2350), [#&#8203;2353](https://redirect.github.com/microsoft/windows-rs/issues/2353)). This allows these collections to be used without requiring a dependency on the larger `windows` crate. This crate also provides an optimized implementation of the standard `Iterator` trait for the Windows `IIterator<T>` interface ([#&#8203;3476](https://redirect.github.com/microsoft/windows-rs/issues/3476)).

- The [windows-future](https://crates.io/crates/windows-future) crate defines the Windows async types like `IAsyncAction`, `IAsyncOperation<T>`, and so on ([#&#8203;3490](https://redirect.github.com/microsoft/windows-rs/issues/3490)). It also includes all of the stock implementations for creating such async types ([#&#8203;3221](https://redirect.github.com/microsoft/windows-rs/issues/3221), [#&#8203;3235](https://redirect.github.com/microsoft/windows-rs/issues/3235)). This allows these async types to be used without requiring a dependency on the larger `windows` crate.

- The [windows-link](https://crates.io/crates/windows-link) crate provides linker support for Windows ([#&#8203;3450](https://redirect.github.com/microsoft/windows-rs/issues/3450)). This is the evolution of the older `windows-targets` crate but is substantially simpler and more versatile thanks to advances in the Rust compiler since the `windows-targets` crate was unveiled. Notably, it does not depend on or insert any import libs and can be used with custom libraries, not only those provided by the Windows operating system. All of the crates, with the exception of `windows-sys`, now depend on the new `windows-link` crate instead of the older `windows-targets` crate. This greatly simplifies compilation and also greatly reduces the size of dependencies as the `windows-link` crate is tiny. The `windows-bindgen` crate defaults to `windows-link` but also adds the `--link` option to override this as needed. You may for example want to use `--link windows_targets` if you need to stick with the `windows-targets` crate if you cannot change your MSRV to Rust 1.71 or later as that was the first version to stabilize `raw-dylib` for all Windows targets. This then lets you continue to use `windows-bindgen` until you are ready to move to a newer version of Rust.

- The [windows-numerics](https://crates.io/crates/windows-numerics) crate defines the Windows numeric types to support graphics-oriented math APIs and calculations ([#&#8203;3488](https://redirect.github.com/microsoft/windows-rs/issues/3488)). It also also includes all of the stock implementations for overloaded operators and other transformations. This allows these numeric types to be used without requiring a dependency on the larger `windows` crate.

##### Major updates to existing crates

- The [windows-bindgen](https://crates.io/crates/windows-bindgen) crate provides a number of improvements including new diagnostics ([#&#8203;3498](https://redirect.github.com/microsoft/windows-rs/issues/3498)), streamlined and more capable reference support ([#&#8203;3497](https://redirect.github.com/microsoft/windows-rs/issues/3497), [#&#8203;3492](https://redirect.github.com/microsoft/windows-rs/issues/3492)), hardened method overloading ([#&#8203;3477](https://redirect.github.com/microsoft/windows-rs/issues/3477)), far fewer `transmute` calls, as well as many other critical fixes and improvements.

- The [windows-core](https://crates.io/crates/windows-core) crate is largely unchanged but required some breaking changes to support `windows-bindgen` type system improvements.

- The [windows-registry](https://crates.io/crates/windows-registry) crate continues to improve with generalized support for access rights ([#&#8203;3482](https://redirect.github.com/microsoft/windows-rs/issues/3482)), open options ([#&#8203;3461](https://redirect.github.com/microsoft/windows-rs/issues/3461)), and other minor improvements.

- The [windows](https://crates.io/crates/windows) crate now delegates to the `windows-numerics`, `windows-future`, and `windows-collections` crates for those types, as well as a number of critical fixes and improvements to features and `cfg` guards ([#&#8203;3431](https://redirect.github.com/microsoft/windows-rs/issues/3431)), and many other small improvements.

##### Minor updates to existing crates

- The [windows-result](https://crates.io/crates/windows-result) now includes the `BOOL` type ([#&#8203;3441](https://redirect.github.com/microsoft/windows-rs/issues/3441)) as a core type. This allows this ubiquitous type to be used without requiring a dependency on the larger `windows` crate.

- The [windows-strings](https://crates.io/crates/windows-strings) crate now  depends on the new `windows-link` crate instead of the older `windows-targets` crate.

- The [windows-version](https://crates.io/crates/windows-version) crate now  depends on the new `windows-link` crate instead of the older `windows-targets` crate.

- The [cppwinrt](https://crates.io/crates/cppwinrt) crate includes minor improvements to improve build reliability.

##### What's Changed

- Use `track_caller` in more places by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3424](https://redirect.github.com/microsoft/windows-rs/pull/3424)
- Allow name and value types in `set_string` to differ by [@&#8203;kerosina](https://redirect.github.com/kerosina) in [#&#8203;3412](https://redirect.github.com/microsoft/windows-rs/pull/3412)
- Simplify internal `windows-bindgen` caching by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3427](https://redirect.github.com/microsoft/windows-rs/pull/3427)
- Fix test build reliability by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3429](https://redirect.github.com/microsoft/windows-rs/pull/3429)
- Use dedicated arch `cfg` writer by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3430](https://redirect.github.com/microsoft/windows-rs/pull/3430)
- Fix `cfg` generation by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3431](https://redirect.github.com/microsoft/windows-rs/pull/3431)
- Remove `Ref` unused lifetime type parameter by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3433](https://redirect.github.com/microsoft/windows-rs/pull/3433)
- Use `Ref` for generic type parameters by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3435](https://redirect.github.com/microsoft/windows-rs/pull/3435)
- Apply type `cfg` to `Send` and `Sync` implementations by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3438](https://redirect.github.com/microsoft/windows-rs/pull/3438)
- Make `BOOL` a core type by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3441](https://redirect.github.com/microsoft/windows-rs/pull/3441)
- Remap the Win32 definition of `EventRegistrationToken` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3445](https://redirect.github.com/microsoft/windows-rs/pull/3445)
- Use `Ref` and `OutRef` for C++ delegates by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3447](https://redirect.github.com/microsoft/windows-rs/pull/3447)
- Reintroduce `Ref` lifetime type parameter by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3448](https://redirect.github.com/microsoft/windows-rs/pull/3448)
- Introducing the `windows-link` crate by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3450](https://redirect.github.com/microsoft/windows-rs/pull/3450)
- Avoid over-wrapping optional callback parameters by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3452](https://redirect.github.com/microsoft/windows-rs/pull/3452)
- Use unique path for `cppwinrt` temp file by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3455](https://redirect.github.com/microsoft/windows-rs/pull/3455)
- Share result mapping code in `windows-bindgen` and reduce `transmute` count by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3454](https://redirect.github.com/microsoft/windows-rs/pull/3454)
- Simplify signature parameter handling inside `windows-bindgen` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3456](https://redirect.github.com/microsoft/windows-rs/pull/3456)
- Remove workaround for invalid metadata by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3457](https://redirect.github.com/microsoft/windows-rs/pull/3457)
- Simplify dependency tracking by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3460](https://redirect.github.com/microsoft/windows-rs/pull/3460)
- Add `OpenOptions` to `windows-registry` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3461](https://redirect.github.com/microsoft/windows-rs/pull/3461)
- Add `windows-link` to the windows-rs readme by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3462](https://redirect.github.com/microsoft/windows-rs/pull/3462)
- Ensure remaining extensions compile with `no_std` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3469](https://redirect.github.com/microsoft/windows-rs/pull/3469)
- Harden nested type discovery by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3471](https://redirect.github.com/microsoft/windows-rs/pull/3471)
- Add collection interop testing by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3473](https://redirect.github.com/microsoft/windows-rs/pull/3473)
- Optimize `Iterator` for `IIterator<T>` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3476](https://redirect.github.com/microsoft/windows-rs/pull/3476)
- Harden method overload support by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3477](https://redirect.github.com/microsoft/windows-rs/pull/3477)
- Generalize support for access rights in `windows-registry` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3482](https://redirect.github.com/microsoft/windows-rs/pull/3482)
- Support metadata with `MethodDef` constructor parent resolution by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3484](https://redirect.github.com/microsoft/windows-rs/pull/3484)
- Introducing the dedicated `windows-collections` crate by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3483](https://redirect.github.com/microsoft/windows-rs/pull/3483)
- Deduplicate required interfaces by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3487](https://redirect.github.com/microsoft/windows-rs/pull/3487)
- Introducing the dedicated `windows-numerics` crate by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3488](https://redirect.github.com/microsoft/windows-rs/pull/3488)
- Introducing the dedicated `windows-future` crate by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3490](https://redirect.github.com/microsoft/windows-rs/pull/3490)
- Improve `windows-bindgen` reference usability and default reference support by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3492](https://redirect.github.com/microsoft/windows-rs/pull/3492)
- Avoid memory explosion when stress testing `bindgen` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3496](https://redirect.github.com/microsoft/windows-rs/pull/3496)
- Simpler default reference support in `windows-bindgen` by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3497](https://redirect.github.com/microsoft/windows-rs/pull/3497)
- Add optional warnings for `windows-bindgen` to improve diagnostics by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3498](https://redirect.github.com/microsoft/windows-rs/pull/3498)
- Organize library tests by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3499](https://redirect.github.com/microsoft/windows-rs/pull/3499)
- Optimize test coverage by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3500](https://redirect.github.com/microsoft/windows-rs/pull/3500)
- Optimize external packages by [@&#8203;sivadeilra](https://redirect.github.com/sivadeilra) in [#&#8203;3501](https://redirect.github.com/microsoft/windows-rs/pull/3501)
- Release 0.62.0 by [@&#8203;kennykerr](https://redirect.github.com/kennykerr) in [#&#8203;3502](https://redirect.github.com/microsoft/windows-rs/pull/3502)

##### New Contributors

- [@&#8203;kerosina](https://redirect.github.com/kerosina) made their first contribution in [#&#8203;3412](https://redirect.github.com/microsoft/windows-rs/pull/3412)

**Full Changelog**: <https://github.com/microsoft/windows-rs/compare/0.61.0...0.62.0>

</details>

---

### Configuration

📅 **Schedule**: Branch creation - Between 12:00 AM and 03:59 AM, only on Monday ( * 0-3 * * 1 ) (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about these updates again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi43NC41IiwidXBkYXRlZEluVmVyIjoiNDIuODUuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiYnVpbGQ6c2tpcC1yZWxlYXNlIiwiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-19 01:35_

---

_Comment by @renovate[bot] on 2026-01-19 01:35_

### ⚠️ Artifact update problem

Renovate failed to update artifacts related to this branch. You probably do not want to merge this PR as-is.

♻ Renovate will retry this branch, including artifacts, only when one of the following happens:

 - any of the package files in this branch needs updating, or 
 - the branch becomes conflicted, or
 - you click the rebase/retry checkbox if found above, or
 - you rename this PR's title to start with "rebase!" to trigger it manually

The artifact failure details are included below:

##### File name: Cargo.lock

```
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --package windows@0.61.3 --precise 0.62.2
    Updating crates.io index
error: failed to select a version for the requirement `windows = "^0.61"`
candidate versions found which didn't match: 0.62.2
location searched: crates.io index
required by package `wmi v0.16.0`
    ... which satisfies dependency `wmi = "^0.16.0"` of package `uv-torch v0.0.15 (/tmp/renovate/repos/github/astral-sh/uv/crates/uv-torch)`
    ... which satisfies path dependency `uv-torch` (locked to 0.0.15) of package `uv v0.9.26 (/tmp/renovate/repos/github/astral-sh/uv/crates/uv)`

```

##### File name: crates/uv-trampoline/Cargo.lock

```
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path crates/uv-trampoline/Cargo.toml --package windows@0.61.3 --precise 0.62.2
error: failed to parse manifest at `/tmp/renovate/repos/github/astral-sh/uv/crates/uv-trampoline/Cargo.toml`

Caused by:
  the cargo feature `panic-immediate-abort` requires a nightly version of Cargo, but this is the `stable` channel
  See https://doc.rust-lang.org/book/appendix-07-nightly-rust.html for more information about Rust release channels.
  See https://doc.rust-lang.org/cargo/reference/unstable.html#panic-immediate-abort for more information about using this feature.

```



---

_Comment by @zanieb on 2026-01-19 05:39_

Conflicts with #17552 

---

_@zanieb reviewed on 2026-01-19 06:03_

---

_Review comment by @zanieb on `crates/uv-python/src/windows_registry.rs`:228 on 2026-01-19 06:03_

I'm not sure if this is needed, to check.

---

_@zanieb reviewed on 2026-01-19 06:03_

---

_Review comment by @zanieb on `crates/uv-python/src/windows_registry.rs`:228 on 2026-01-19 06:03_

See https://github.com/astral-sh/uv/pull/17601#discussion_r2703284194

---

_Label `build:skip-release` added by @renovate[bot] on 2026-01-19 23:16_

---
