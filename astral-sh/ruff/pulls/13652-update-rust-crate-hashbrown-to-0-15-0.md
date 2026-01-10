```yaml
number: 13652
title: Update Rust crate hashbrown to 0.15.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/hashbrown-0.x
created_at: 2024-10-07T01:12:30Z
updated_at: 2024-10-07T07:02:24Z
url: https://github.com/astral-sh/ruff/pull/13652
synced_at: 2026-01-10T20:59:36Z
```

# Update Rust crate hashbrown to 0.15.0

---

_Pull request opened by @renovate on 2024-10-07 01:12_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [hashbrown](https://redirect.github.com/rust-lang/hashbrown) | workspace.dependencies | minor | `0.14.3` -> `0.15.0` |

---

### Release Notes

<details>
<summary>rust-lang/hashbrown (hashbrown)</summary>

### [`v0.15.0`](https://redirect.github.com/rust-lang/hashbrown/blob/HEAD/CHANGELOG.md#v0150---2024-10-01)

[Compare Source](https://redirect.github.com/rust-lang/hashbrown/compare/v0.14.5...v0.15.0)

This update contains breaking changes that remove the `raw` API with the hope of
centralising on the `HashTable` API in the future. You can follow the discussion
and progress in [#&#8203;545](https://redirect.github.com/rust-lang/hashbrown/issues/545) to discuss features you think should be added to this API
that were previously only possible on the `raw` API.

##### Added

-   Added `borsh` feature with `BorshSerialize` and `BorshDeserialize` impls. ([#&#8203;525](https://redirect.github.com/rust-lang/hashbrown/issues/525))
-   Added `Assign` impls for `HashSet` operators. ([#&#8203;529](https://redirect.github.com/rust-lang/hashbrown/issues/529))
-   Added `Default` impls for iterator types. ([#&#8203;542](https://redirect.github.com/rust-lang/hashbrown/issues/542))
-   Added `HashTable::iter_hash{,_mut}` methods. ([#&#8203;549](https://redirect.github.com/rust-lang/hashbrown/issues/549))
-   Added `Hash{Table,Map,Set}::allocation_size` methods. ([#&#8203;553](https://redirect.github.com/rust-lang/hashbrown/issues/553))
-   Implemented `Debug` and `FusedIterator` for all `HashTable` iterators. ([#&#8203;561](https://redirect.github.com/rust-lang/hashbrown/issues/561))
-   Specialized `Iterator::fold` for all `HashTable` iterators. ([#&#8203;561](https://redirect.github.com/rust-lang/hashbrown/issues/561))

##### Changed

-   Changed `hash_set::VacantEntry::insert` to return `OccupiedEntry`. ([#&#8203;495](https://redirect.github.com/rust-lang/hashbrown/issues/495))
-   Improved`hash_set::Difference::size_hint` lower-bound. ([#&#8203;530](https://redirect.github.com/rust-lang/hashbrown/issues/530))
-   Improved `HashSet::is_disjoint` performance. ([#&#8203;531](https://redirect.github.com/rust-lang/hashbrown/issues/531))
-   `equivalent` feature is now enabled by default. ([#&#8203;532](https://redirect.github.com/rust-lang/hashbrown/issues/532))
-   `HashSet` operators now return a set with the same allocator. ([#&#8203;529](https://redirect.github.com/rust-lang/hashbrown/issues/529))
-   Changed the default hasher to foldhash. ([#&#8203;563](https://redirect.github.com/rust-lang/hashbrown/issues/563))
-   `ahash` feature has been renamed to `default-hasher`. ([#&#8203;533](https://redirect.github.com/rust-lang/hashbrown/issues/533))
-   Entry API has been reworked and several methods have been renamed. ([#&#8203;535](https://redirect.github.com/rust-lang/hashbrown/issues/535))
-   `Hash{Map,Set}::insert_unique_unchecked` is now unsafe. ([#&#8203;556](https://redirect.github.com/rust-lang/hashbrown/issues/556))
-   The signature of `get_many_mut` and related methods was changed. ([#&#8203;562](https://redirect.github.com/rust-lang/hashbrown/issues/562))

##### Fixed

-   Fixed typos, stray backticks in docs. ([#&#8203;558](https://redirect.github.com/rust-lang/hashbrown/issues/558), [#&#8203;560](https://redirect.github.com/rust-lang/hashbrown/issues/560))

##### Removed

-   Raw entry API is now under `raw-entry` feature, to be eventually removed. ([#&#8203;534](https://redirect.github.com/rust-lang/hashbrown/issues/534), [#&#8203;555](https://redirect.github.com/rust-lang/hashbrown/issues/555))
-   Raw table API has been made private and the `raw` feature is removed;
    in the future, all code should be using the `HashTable` API instead. ([#&#8203;531](https://redirect.github.com/rust-lang/hashbrown/issues/531), [#&#8203;546](https://redirect.github.com/rust-lang/hashbrown/issues/546))
-   `rykv` feature was removed; this is now provided by the `rykv` crate instead. ([#&#8203;554](https://redirect.github.com/rust-lang/hashbrown/issues/554))
-   `HashSet::get_or_insert_owned` was removed in favor of `get_or_insert_with`. ([#&#8203;555](https://redirect.github.com/rust-lang/hashbrown/issues/555))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC45Ny4wIiwidXBkYXRlZEluVmVyIjoiMzguOTcuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Comment by @renovate[bot] on 2024-10-07 01:12_

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
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --package hashbrown@0.14.5 --precise 0.15.0
    Updating crates.io index
error: failed to select a version for the requirement `hashbrown = "^0.14.0"`
candidate versions found which didn't match: 0.15.0
location searched: crates.io index
required by package `dashmap v6.1.0`
    ... which satisfies dependency `dashmap = "^6.0.1"` (locked to 6.1.0) of package `ruff_db v0.0.0 (/tmp/renovate/repos/github/astral-sh/ruff/crates/ruff_db)`
    ... which satisfies path dependency `ruff_db` (locked to 0.0.0) of package `red_knot v0.0.0 (/tmp/renovate/repos/github/astral-sh/ruff/crates/red_knot)`

```



---

_Label `internal` added by @renovate[bot] on 2024-10-07 01:12_

---

_Comment by @github-actions[bot] on 2024-10-07 01:32_

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

_Review requested from @carljm by @MichaReiser on 2024-10-07 06:42_

---

_Review requested from @MichaReiser by @MichaReiser on 2024-10-07 06:42_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-07 06:42_

---

_@MichaReiser approved on 2024-10-07 06:43_

---

_Merged by @MichaReiser on 2024-10-07 06:50_

---

_Closed by @MichaReiser on 2024-10-07 06:50_

---

_Branch deleted on 2024-10-07 06:51_

---
