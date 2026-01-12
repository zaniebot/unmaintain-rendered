```yaml
number: 7415
title: Update Rust crate rkyv to 0.8.0
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/rkyv-0.x
created_at: 2024-09-16T00:23:14Z
updated_at: 2024-09-18T18:50:31Z
url: https://github.com/astral-sh/uv/pull/7415
synced_at: 2026-01-12T16:07:49Z
```

# Update Rust crate rkyv to 0.8.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [rkyv](https://redirect.github.com/rkyv/rkyv) | workspace.dependencies | minor | `0.7.45` -> `0.8.0` |

---

### Release Notes

<details>
<summary>rkyv/rkyv (rkyv)</summary>

### [`v0.8.8`](https://redirect.github.com/rkyv/rkyv/releases/tag/0.8.8)

[Compare Source](https://redirect.github.com/rkyv/rkyv/compare/0.8.7...0.8.8)

-   Fixes auto copy optimization referring to generated types with `as = ..`.

**Full Changelog**: https://github.com/rkyv/rkyv/compare/0.8.7...0.8.8

### [`v0.8.7`](https://redirect.github.com/rkyv/rkyv/releases/tag/0.8.7)

[Compare Source](https://redirect.github.com/rkyv/rkyv/compare/0.8.6...0.8.7)

-   Removes an unused lifetime from the high- and low-level API serializer types.

**Full Changelog**: https://github.com/rkyv/rkyv/compare/0.8.6...0.8.7

### [`v0.8.6`](https://redirect.github.com/rkyv/rkyv/releases/tag/0.8.6)

[Compare Source](https://redirect.github.com/rkyv/rkyv/compare/0.8.5...0.8.6)

#### What's Changed

-   Derive Default, Eq, Hash, Ord, PartialEq, and PartialOrd for ArchivedTuple\* by [@&#8203;evie-calico](https://redirect.github.com/evie-calico) in [https://github.com/rkyv/rkyv/pull/556](https://redirect.github.com/rkyv/rkyv/pull/556)

#### New Contributors

-   [@&#8203;evie-calico](https://redirect.github.com/evie-calico) made their first contribution in [https://github.com/rkyv/rkyv/pull/556](https://redirect.github.com/rkyv/rkyv/pull/556)

**Full Changelog**: https://github.com/rkyv/rkyv/compare/0.8.5...0.8.6

### [`v0.8.5`](https://redirect.github.com/rkyv/rkyv/releases/tag/0.8.5)

[Compare Source](https://redirect.github.com/rkyv/rkyv/compare/0.8.4...0.8.5)

This bugfix release only includes performance optimizations.

**Full Changelog**: https://github.com/rkyv/rkyv/compare/0.8.4...0.8.5

### [`v0.8.4`](https://redirect.github.com/rkyv/rkyv/releases/tag/0.8.4)

[Compare Source](https://redirect.github.com/rkyv/rkyv/compare/0.8.3...0.8.4)

#### What's Changed

-   Support missing enum variants on remote derives by [@&#8203;MaxOhn](https://redirect.github.com/MaxOhn) in [https://github.com/rkyv/rkyv/pull/553](https://redirect.github.com/rkyv/rkyv/pull/553)
-   Removed a stale reference to `ArchivedAlignedVec`
-   Fixed a panic caused by serializing hashmaps of large elements
-   Reduced branching when writing inline strings (~15% ser improvement on log benchmark)

**Full Changelog**: https://github.com/rkyv/rkyv/compare/0.8.3...0.8.4

### [`v0.8.3`](https://redirect.github.com/rkyv/rkyv/releases/tag/0.8.3)

[Compare Source](https://redirect.github.com/rkyv/rkyv/compare/0.8.2...0.8.3)

#### What's Changed

-   fix: minimum `syn` version by [@&#8203;xJonathanLEI](https://redirect.github.com/xJonathanLEI) in [https://github.com/rkyv/rkyv/pull/550](https://redirect.github.com/rkyv/rkyv/pull/550)

#### New Contributors

-   [@&#8203;xJonathanLEI](https://redirect.github.com/xJonathanLEI) made their first contribution in [https://github.com/rkyv/rkyv/pull/550](https://redirect.github.com/rkyv/rkyv/pull/550)

**Full Changelog**: https://github.com/rkyv/rkyv/compare/0.8.2...0.8.3

### [`v0.8.2`](https://redirect.github.com/rkyv/rkyv/releases/tag/0.8.2)

[Compare Source](https://redirect.github.com/rkyv/rkyv/compare/0.8.1...0.8.2)

This bugfix release addresses the following issues:

-   [#&#8203;548](https://redirect.github.com/rkyv/rkyv/issues/548) Makes the `AsVec` wrapper compatible with any choice of hasher
-   [#&#8203;549](https://redirect.github.com/rkyv/rkyv/issues/549) Access pointer validation fails for nested `HashMap`s

### [`v0.8.1`](https://redirect.github.com/rkyv/rkyv/releases/tag/0.8.1)

[Compare Source](https://redirect.github.com/rkyv/rkyv/compare/0.8.0...0.8.1)

This bugfix release corrects an infinite loop in hash map probing.

### [`v0.8.0`](https://redirect.github.com/rkyv/rkyv/releases/tag/0.8.0)

It's finally here! A ton of stuff has changed, so here are some highlights:

-   API free functions are now more ergonomic and consistently-named. See `to_bytes`, `from_bytes`, `access`, and everything else!
-   rkyv now provides separate "API levels" for high-level Rust code and low-level Rust code.
-   Error handling has been completely overhauled with the introduction of rancor. Validation, serialization, and deserialization all now accept error type parameters so you can choose how you want errors to accumulate.
-   rkyv now supports remote derive! Read all about it in [the book](https://rkyv.org/derive-macro-features/remote-derive.html).
-   Unaligned primitives are now supported via the `unaligned` feature. If you choose, no more worrying about buffer alignment!
-   Native endianness is no more. rkyv now defaults to little-endian, aligned, and 32-bit relative pointers. Use the format control features to change them if you want.
-   A few semver-affecting soundness issues have been fixed. rkyv should now really truly and always generate cross-platform buffers.
-   The archived hash map and b-tree map implementations have been overhauled for better space-efficiency and performance.
-   Macro attributes have been overhauled to make them significantly more cohesive and ergonomic.
-   rkyv's mutable API has been overhauled for improved soundness and ease of use.
-   rkyv's internal APIs have been overhauled for improved ease of use.
-   Many serialization and deserialization types and traits have been renamed for shortness and clarity.
-   Copy optimizations are now stable for many basic primitive types.
-   The Most Unhelpful Error no longer occurs (the compiler mentions `With<_, _>` when you try to deserialize)
-   `AlignedVec` now supports custom alignments
-   No more `strict` feature, rkyv is always in strict mode now
-   Lots and lots and lots of tech debt cleanup

... and much more! Try it out yourself after I release this new version on-stage at RustConf.

Reminder: this is a major version bump, and data previously serialized with rkyv 0.7 or below will not be compatible with 0.8. Any data you serialize in 0.8 is guaranteed to be compatible for the lifetime of the 0.8 releases.

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC43NC4xIiwidXBkYXRlZEluVmVyIjoiMzguODAuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-09-16 00:23_

---

_Comment by @renovate[bot] on 2024-09-16 00:23_

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
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --workspace
    Updating git repository `https://github.com/astral-sh/reqwest-middleware`
From https://github.com/astral-sh/reqwest-middleware
 * [new ref]         5e3eaf254b5bd481c75d2710eed055f95b756913 -> refs/commit/5e3eaf254b5bd481c75d2710eed055f95b756913
    Updating crates.io index
    Updating git repository `https://github.com/astral-sh/pubgrub`
From https://github.com/astral-sh/pubgrub
 * [new ref]         388685a8711092971930986644cfed152d1a1f6c -> refs/commit/388685a8711092971930986644cfed152d1a1f6c
 * [new tag]         perma-45   -> perma-45
 * [new tag]         perma-0    -> perma-0
    Updating git repository `https://github.com/astral-sh/reqwest-middleware`
    Updating git repository `https://github.com/charliermarsh/rs-async-zip`
From https://github.com/charliermarsh/rs-async-zip
 * [new ref]         011b24604fa7bc223daaad7712c0694bac8f0a87 -> refs/commit/011b24604fa7bc223daaad7712c0694bac8f0a87
 * [new tag]         v0.0.10    -> v0.0.10
 * [new tag]         v0.0.11    -> v0.0.11
 * [new tag]         v0.0.16    -> v0.0.16
 * [new tag]         v0.0.17    -> v0.0.17
 * [new tag]         v0.0.2     -> v0.0.2
 * [new tag]         v0.0.3     -> v0.0.3
 * [new tag]         v0.0.4     -> v0.0.4
 * [new tag]         v0.0.5     -> v0.0.5
 * [new tag]         v0.0.6     -> v0.0.6
 * [new tag]         v0.0.7     -> v0.0.7
 * [new tag]         v0.0.8     -> v0.0.8
 * [new tag]         v0.0.9     -> v0.0.9
    Updating git repository `https://github.com/charliermarsh/tl.git`
From https://github.com/charliermarsh/tl
 * [new ref]         6e25b2ee2513d75385101a8ff9f591ef51f314ec -> refs/commit/6e25b2ee2513d75385101a8ff9f591ef51f314ec
 * [new tag]         6e25b2     -> 6e25b2
error: failed to select a version for `rkyv`.
    ... required by package `distribution-filename v0.0.1 (/tmp/renovate/repos/github/astral-sh/uv/crates/distribution-filename)`
    ... which satisfies path dependency `distribution-filename` (locked to 0.0.1) of package `bench v0.0.0 (/tmp/renovate/repos/github/astral-sh/uv/crates/bench)`
versions that meet the requirements `^0.8.0` are: 0.8.5, 0.8.4, 0.8.3, 0.8.2, 0.8.1, 0.8.0

the package `distribution-filename` depends on `rkyv`, with features: `strict` but `rkyv` does not have these features.


failed to select a version for `rkyv` which could resolve this conflict

```



---

_Assigned to @BurntSushi by @zanieb on 2024-09-16 01:09_

---

_Closed by @BurntSushi on 2024-09-18 18:49_

---

_Closed by @BurntSushi on 2024-09-18 18:49_

---

_Branch deleted on 2024-09-18 18:50_

---
