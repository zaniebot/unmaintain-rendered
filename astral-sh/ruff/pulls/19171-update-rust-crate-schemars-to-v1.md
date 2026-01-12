```yaml
number: 19171
title: Update Rust crate schemars to v1
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/schemars-1.x
created_at: 2025-07-07T03:25:23Z
updated_at: 2025-08-10T14:21:09Z
url: https://github.com/astral-sh/ruff/pull/19171
synced_at: 2026-01-12T15:56:33Z
```

# Update Rust crate schemars to v1

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [schemars](https://graham.cool/schemars/) ([source](https://redirect.github.com/GREsau/schemars)) | workspace.dependencies | major | `0.8.16` -> `1.0.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>GREsau/schemars (schemars)</summary>

### [`v1.0.4`](https://redirect.github.com/GREsau/schemars/blob/HEAD/CHANGELOG.md#104---2025-07-06)

[Compare Source](https://redirect.github.com/GREsau/schemars/compare/v1.0.3...v1.0.4)

##### Fixed

- Fix `JsonSchema` impl on [atomic](https://doc.rust-lang.org/std/sync/atomic/) types being ignored on non-nightly compilers due to a buggy `cfg` check ([https://github.com/GREsau/schemars/issues/453](https://redirect.github.com/GREsau/schemars/issues/453))
- Fix compatibility with minimal dependency versions, e.g. old(-ish) versions of `syn` ([https://github.com/GREsau/schemars/issues/450](https://redirect.github.com/GREsau/schemars/issues/450))
- Fix derive for empty tuple variants ([https://github.com/GREsau/schemars/issues/455](https://redirect.github.com/GREsau/schemars/issues/455))

### [`v1.0.3`](https://redirect.github.com/GREsau/schemars/blob/HEAD/CHANGELOG.md#103---2025-06-28)

[Compare Source](https://redirect.github.com/GREsau/schemars/compare/v1.0.2...v1.0.3)

##### Fixed

- Fix compile error when a doc comment is set on both a `transparent` (or newtype) struct and its field ([https://github.com/GREsau/schemars/issues/446](https://redirect.github.com/GREsau/schemars/issues/446))
- Fix `json_schema!()` macro compatibility when used from pre-2021 rust editions ([https://github.com/GREsau/schemars/pull/447](https://redirect.github.com/GREsau/schemars/pull/447))

### [`v1.0.2`](https://redirect.github.com/GREsau/schemars/blob/HEAD/CHANGELOG.md#102---2025-06-26)

[Compare Source](https://redirect.github.com/GREsau/schemars/compare/v1.0.1...v1.0.2)

##### Fixed

- Fix schema properties being incorrectly reordered during serialization ([https://github.com/GREsau/schemars/issues/444](https://redirect.github.com/GREsau/schemars/issues/444))

### [`v1.0.1`](https://redirect.github.com/GREsau/schemars/blob/HEAD/CHANGELOG.md#101---2025-06-24)

[Compare Source](https://redirect.github.com/GREsau/schemars/compare/v1.0.0...v1.0.1)

##### Fixed

- Deriving `JsonSchema` with `no_std` broken due to `std::borrow::ToOwned` trait not being in scope ([https://github.com/GREsau/schemars/issues/441](https://redirect.github.com/GREsau/schemars/issues/441))

### [`v1.0.0`](https://redirect.github.com/GREsau/schemars/blob/HEAD/CHANGELOG.md#100---2025-06-23)

[Compare Source](https://redirect.github.com/GREsau/schemars/compare/v0.9.0...v1.0.0)

This is a major release with many additions, fixes and changes since 0.8 (but not many since 0.9). While the basic usage (deriving `JsonSchema` and using `schema_for!()` or `SchemaGenerator`) is mostly unchanged, you may wish to consult the [migration guide](https://graham.cool/schemars/migrating/) which covers some of the most significant changes.

Changes since 1.0.0-rc.2:

##### Added

- `#[schemars(bound = ...)]` attributes are now used from fields as well as containers
- The [`Schema::pointer(...)`](https://docs.rs/schemars/1.0.0/schemars/struct.Schema.html#method.pointer) method now works when given a JSON pointer in URI Fragment representation with a leading `#` character. In particular, this means that you can now lookup a schema from a `$ref` value using that method.

##### Fixed

- Schema names that contain special characters are now correctly encoded when used inside a `$ref` value ([https://github.com/GREsau/schemars/pull/436](https://redirect.github.com/GREsau/schemars/pull/436))
- Optimise type param usage in `SchemaGenerator::subschema_for`, reducing LLVM line count and improving compile times ([https://github.com/GREsau/schemars/pull/439](https://redirect.github.com/GREsau/schemars/pull/439))

### [`v0.9.0`](https://redirect.github.com/GREsau/schemars/blob/HEAD/CHANGELOG.md#090---2025-05-26)

[Compare Source](https://redirect.github.com/GREsau/schemars/compare/v0.8.22...v0.9.0)

This version is identical to `1.0.0-alpha.18`, but is available for those who are unable to unwilling to use a pre-release version.

Those upgrading from Schemars 0.8 may want to consult [the migration guide](https://graham.cool/schemars/migrating/), which also applies when migrating from 0.8 to 0.9.

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4xNy4yIiwidXBkYXRlZEluVmVyIjoiNDEuMTcuMiIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-07-07 03:25_

---

_Closed by @MichaReiser on 2025-08-10 14:19_

---

_Comment by @renovate[bot] on 2025-08-10 14:21_

### Renovate Ignore Notification

Because you closed this PR without merging, Renovate will ignore this update. You will not get PRs for *any* future `1.x` releases. But if you manually upgrade to `1.x` then Renovate will re-enable `minor` and `patch` updates automatically.

If you accidentally closed this PR, or if you changed your mind: rename this PR to get a fresh replacement PR.

---

_Branch deleted on 2025-08-10 14:21_

---
