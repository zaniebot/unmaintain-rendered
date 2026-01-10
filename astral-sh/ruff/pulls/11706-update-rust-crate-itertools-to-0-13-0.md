```yaml
number: 11706
title: Update Rust crate itertools to 0.13.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/itertools-0.x
created_at: 2024-06-03T01:00:11Z
updated_at: 2024-06-03T01:22:17Z
url: https://github.com/astral-sh/ruff/pull/11706
synced_at: 2026-01-10T21:56:00Z
```

# Update Rust crate itertools to 0.13.0

---

_Pull request opened by @renovate on 2024-06-03 01:00_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [itertools](https://togithub.com/rust-itertools/itertools) | workspace.dependencies | minor | `0.12.1` -> `0.13.0` |

---

### Release Notes

<details>
<summary>rust-itertools/itertools (itertools)</summary>

### [`v0.13.0`](https://togithub.com/rust-itertools/itertools/blob/HEAD/CHANGELOG.md#0130)

[Compare Source](https://togithub.com/rust-itertools/itertools/compare/v0.12.1...v0.13.0)

##### Breaking

-   Removed implementation of `DoubleEndedIterator` for `ConsTuples` ([#&#8203;853](https://togithub.com/rust-itertools/itertools/issues/853))
-   Made `MultiProduct` fused and fixed on an empty iterator ([#&#8203;835](https://togithub.com/rust-itertools/itertools/issues/835), [#&#8203;834](https://togithub.com/rust-itertools/itertools/issues/834))
-   Changed `iproduct!` to return tuples for maxi one iterator too ([#&#8203;870](https://togithub.com/rust-itertools/itertools/issues/870))
-   Changed `PutBack::put_back` to return the old value ([#&#8203;880](https://togithub.com/rust-itertools/itertools/issues/880))
-   Removed deprecated `repeat_call, Itertools::{foreach, step, map_results, fold_results}` ([#&#8203;878](https://togithub.com/rust-itertools/itertools/issues/878))
-   Removed `TakeWhileInclusive::new` ([#&#8203;912](https://togithub.com/rust-itertools/itertools/issues/912))

##### Added

-   Added `Itertools::{smallest_by, smallest_by_key, largest, largest_by, largest_by_key}` ([#&#8203;654](https://togithub.com/rust-itertools/itertools/issues/654), [#&#8203;885](https://togithub.com/rust-itertools/itertools/issues/885))
-   Added `Itertools::tail` ([#&#8203;899](https://togithub.com/rust-itertools/itertools/issues/899))
-   Implemented `DoubleEndedIterator` for `ProcessResults` ([#&#8203;910](https://togithub.com/rust-itertools/itertools/issues/910))
-   Implemented `Debug` for `FormatWith` ([#&#8203;931](https://togithub.com/rust-itertools/itertools/issues/931))
-   Added `Itertools::get` ([#&#8203;891](https://togithub.com/rust-itertools/itertools/issues/891))

##### Changed

-   Deprecated `Itertools::group_by` (renamed `chunk_by`) ([#&#8203;866](https://togithub.com/rust-itertools/itertools/issues/866), [#&#8203;879](https://togithub.com/rust-itertools/itertools/issues/879))
-   Deprecated `unfold` (use `std::iter::from_fn` instead) ([#&#8203;871](https://togithub.com/rust-itertools/itertools/issues/871))
-   Optimized `GroupingMapBy` ([#&#8203;873](https://togithub.com/rust-itertools/itertools/issues/873), [#&#8203;876](https://togithub.com/rust-itertools/itertools/issues/876))
-   Relaxed `Fn` bounds to `FnMut` in `diff_with, Itertools::into_group_map_by` ([#&#8203;886](https://togithub.com/rust-itertools/itertools/issues/886))
-   Relaxed `Debug/Clone` bounds for `MapInto` ([#&#8203;889](https://togithub.com/rust-itertools/itertools/issues/889))
-   Documented the `use_alloc` feature ([#&#8203;887](https://togithub.com/rust-itertools/itertools/issues/887))
-   Optimized `Itertools::set_from` ([#&#8203;888](https://togithub.com/rust-itertools/itertools/issues/888))
-   Removed badges in `README.md` ([#&#8203;890](https://togithub.com/rust-itertools/itertools/issues/890))
-   Added "no-std" categories in `Cargo.toml` ([#&#8203;894](https://togithub.com/rust-itertools/itertools/issues/894))
-   Fixed `Itertools::k_smallest` on short unfused iterators ([#&#8203;900](https://togithub.com/rust-itertools/itertools/issues/900))
-   Deprecated `Itertools::tree_fold1` (renamed `tree_reduce`) ([#&#8203;895](https://togithub.com/rust-itertools/itertools/issues/895))
-   Deprecated `GroupingMap::fold_first` (renamed `reduce`) ([#&#8203;902](https://togithub.com/rust-itertools/itertools/issues/902))
-   Fixed `Itertools::k_smallest(0)` to consume the iterator, optimized `Itertools::k_smallest(1)` ([#&#8203;909](https://togithub.com/rust-itertools/itertools/issues/909))
-   Specialized `Combinations::nth` ([#&#8203;914](https://togithub.com/rust-itertools/itertools/issues/914))
-   Specialized `MergeBy::fold` ([#&#8203;920](https://togithub.com/rust-itertools/itertools/issues/920))
-   Specialized `CombinationsWithReplacement::nth` ([#&#8203;923](https://togithub.com/rust-itertools/itertools/issues/923))
-   Specialized `FlattenOk::{fold, rfold}` ([#&#8203;927](https://togithub.com/rust-itertools/itertools/issues/927))
-   Specialized `Powerset::nth` ([#&#8203;924](https://togithub.com/rust-itertools/itertools/issues/924))
-   Documentation fixes ([#&#8203;882](https://togithub.com/rust-itertools/itertools/issues/882), [#&#8203;936](https://togithub.com/rust-itertools/itertools/issues/936))
-   Fixed `assert_equal` for iterators longer than `i32::MAX` ([#&#8203;932](https://togithub.com/rust-itertools/itertools/issues/932))
-   Updated the `must_use` message of non-lazy `KMergeBy` and `TupleCombinations` ([#&#8203;939](https://togithub.com/rust-itertools/itertools/issues/939))

##### Notable Internal Changes

-   Tested iterator laziness ([#&#8203;792](https://togithub.com/rust-itertools/itertools/issues/792))
-   Created `CONTRIBUTING.md` ([#&#8203;767](https://togithub.com/rust-itertools/itertools/issues/767))

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

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4zNzcuOCIsInVwZGF0ZWRJblZlciI6IjM3LjM3Ny44IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-06-03 01:00_

---

_Merged by @charliermarsh on 2024-06-03 01:05_

---

_Closed by @charliermarsh on 2024-06-03 01:05_

---

_Branch deleted on 2024-06-03 01:05_

---

_Comment by @github-actions[bot] on 2024-06-03 01:22_

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
