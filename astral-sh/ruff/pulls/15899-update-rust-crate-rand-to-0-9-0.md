```yaml
number: 15899
title: Update Rust crate rand to 0.9.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/rand-0.x
created_at: 2025-02-03T00:25:40Z
updated_at: 2025-02-03T11:25:59Z
url: https://github.com/astral-sh/ruff/pull/15899
synced_at: 2026-01-12T15:55:53Z
```

# Update Rust crate rand to 0.9.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [rand](https://rust-random.github.io/book) ([source](https://redirect.github.com/rust-random/rand)) | workspace.dependencies | minor | `0.8.5` -> `0.9.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>rust-random/rand (rand)</summary>

### [`v0.9.0`](https://redirect.github.com/rust-random/rand/blob/HEAD/CHANGELOG.md#090---2025-01-27)

[Compare Source](https://redirect.github.com/rust-random/rand/compare/0.8.5...0.9.0)

##### Security and unsafe

-   Policy: "rand is not a crypto library" ([#&#8203;1514](https://redirect.github.com/rust-random/rand/issues/1514))
-   Remove fork-protection from `ReseedingRng` and `ThreadRng`. Instead, it is recommended to call `ThreadRng::reseed` on fork. ([#&#8203;1379](https://redirect.github.com/rust-random/rand/issues/1379))
-   Use `zerocopy` to replace some `unsafe` code ([#&#8203;1349](https://redirect.github.com/rust-random/rand/issues/1349), [#&#8203;1393](https://redirect.github.com/rust-random/rand/issues/1393), [#&#8203;1446](https://redirect.github.com/rust-random/rand/issues/1446), [#&#8203;1502](https://redirect.github.com/rust-random/rand/issues/1502))

##### Dependencies

-   Bump the MSRV to 1.63.0 ([#&#8203;1207](https://redirect.github.com/rust-random/rand/issues/1207), [#&#8203;1246](https://redirect.github.com/rust-random/rand/issues/1246), [#&#8203;1269](https://redirect.github.com/rust-random/rand/issues/1269), [#&#8203;1341](https://redirect.github.com/rust-random/rand/issues/1341), [#&#8203;1416](https://redirect.github.com/rust-random/rand/issues/1416), [#&#8203;1536](https://redirect.github.com/rust-random/rand/issues/1536)); note that 1.60.0 may work for dependents when using `--ignore-rust-version`
-   Update to `rand_core` v0.9.0 ([#&#8203;1558](https://redirect.github.com/rust-random/rand/issues/1558))

##### Features

-   Support `std` feature without `getrandom` or `rand_chacha` ([#&#8203;1354](https://redirect.github.com/rust-random/rand/issues/1354))
-   Enable feature `small_rng` by default ([#&#8203;1455](https://redirect.github.com/rust-random/rand/issues/1455))
-   Remove implicit feature `rand_chacha`; use `std_rng` instead. ([#&#8203;1473](https://redirect.github.com/rust-random/rand/issues/1473))
-   Rename feature `serde1` to `serde` ([#&#8203;1477](https://redirect.github.com/rust-random/rand/issues/1477))
-   Rename feature `getrandom` to `os_rng` ([#&#8203;1537](https://redirect.github.com/rust-random/rand/issues/1537))
-   Add feature `thread_rng` ([#&#8203;1547](https://redirect.github.com/rust-random/rand/issues/1547))

##### API changes: rand_core traits

-   Add fn `RngCore::read_adapter` implementing `std::io::Read` ([#&#8203;1267](https://redirect.github.com/rust-random/rand/issues/1267))
-   Add trait `CryptoBlockRng: BlockRngCore`; make `trait CryptoRng: RngCore` ([#&#8203;1273](https://redirect.github.com/rust-random/rand/issues/1273))
-   Add traits `TryRngCore`, `TryCryptoRng` ([#&#8203;1424](https://redirect.github.com/rust-random/rand/issues/1424), [#&#8203;1499](https://redirect.github.com/rust-random/rand/issues/1499))
-   Rename `fn SeedableRng::from_rng` -> `try_from_rng` and add infallible variant `fn from_rng` ([#&#8203;1424](https://redirect.github.com/rust-random/rand/issues/1424))
-   Rename `fn SeedableRng::from_entropy` -> `from_os_rng` and add fallible variant `fn try_from_os_rng` ([#&#8203;1424](https://redirect.github.com/rust-random/rand/issues/1424))
-   Add bounds `Clone` and `AsRef` to associated type `SeedableRng::Seed` ([#&#8203;1491](https://redirect.github.com/rust-random/rand/issues/1491))

##### API changes: Rng trait and top-level fns

-   Rename fn `rand::thread_rng()` to `rand::rng()` and remove from the prelude ([#&#8203;1506](https://redirect.github.com/rust-random/rand/issues/1506))
-   Remove fn `rand::random()` from the prelude ([#&#8203;1506](https://redirect.github.com/rust-random/rand/issues/1506))
-   Add top-level fns `random_iter`, `random_range`, `random_bool`, `random_ratio`, `fill` ([#&#8203;1488](https://redirect.github.com/rust-random/rand/issues/1488))
-   Re-introduce fn `Rng::gen_iter` as `random_iter` ([#&#8203;1305](https://redirect.github.com/rust-random/rand/issues/1305), [#&#8203;1500](https://redirect.github.com/rust-random/rand/issues/1500))
-   Rename fn `Rng::gen` to `random` to avoid conflict with the new `gen` keyword in Rust 2024 ([#&#8203;1438](https://redirect.github.com/rust-random/rand/issues/1438))
-   Rename fns `Rng::gen_range` to `random_range`, `gen_bool` to `random_bool`, `gen_ratio` to `random_ratio` ([#&#8203;1505](https://redirect.github.com/rust-random/rand/issues/1505))
-   Annotate panicking methods with `#[track_caller]` ([#&#8203;1442](https://redirect.github.com/rust-random/rand/issues/1442), [#&#8203;1447](https://redirect.github.com/rust-random/rand/issues/1447))

##### API changes: RNGs

-   Fix `<SmallRng as SeedableRng>::Seed` size to 256 bits ([#&#8203;1455](https://redirect.github.com/rust-random/rand/issues/1455))
-   Remove first parameter (`rng`) of `ReseedingRng::new` ([#&#8203;1533](https://redirect.github.com/rust-random/rand/issues/1533))

##### API changes: Sequences

-   Split trait `SliceRandom` into `IndexedRandom`, `IndexedMutRandom`, `SliceRandom` ([#&#8203;1382](https://redirect.github.com/rust-random/rand/issues/1382))
-   Add `IndexedRandom::choose_multiple_array`, `index::sample_array` ([#&#8203;1453](https://redirect.github.com/rust-random/rand/issues/1453), [#&#8203;1469](https://redirect.github.com/rust-random/rand/issues/1469))

##### API changes: Distributions: renames

-   Rename module `rand::distributions` to `rand::distr` ([#&#8203;1470](https://redirect.github.com/rust-random/rand/issues/1470))
-   Rename distribution `Standard` to `StandardUniform` ([#&#8203;1526](https://redirect.github.com/rust-random/rand/issues/1526))
-   Move `distr::Slice` -> `distr::slice::Choose`, `distr::EmptySlice` -> `distr::slice::Empty` ([#&#8203;1548](https://redirect.github.com/rust-random/rand/issues/1548))
-   Rename trait `distr::DistString` -> `distr::SampleString` ([#&#8203;1548](https://redirect.github.com/rust-random/rand/issues/1548))
-   Rename `distr::DistIter` -> `distr::Iter`, `distr::DistMap` -> `distr::Map` ([#&#8203;1548](https://redirect.github.com/rust-random/rand/issues/1548))

##### API changes: Distributions

-   Relax `Sized` bound on `Distribution<T> for &D` ([#&#8203;1278](https://redirect.github.com/rust-random/rand/issues/1278))
-   Remove impl of `Distribution<Option<T>>` for `StandardUniform` ([#&#8203;1526](https://redirect.github.com/rust-random/rand/issues/1526))
-   Let distribution `StandardUniform` support all `NonZero*` types ([#&#8203;1332](https://redirect.github.com/rust-random/rand/issues/1332))
-   Fns `{Uniform, UniformSampler}::{new, new_inclusive}` return a `Result` (instead of potentially panicking) ([#&#8203;1229](https://redirect.github.com/rust-random/rand/issues/1229))
-   Distribution `Uniform` implements `TryFrom` instead of `From` for ranges ([#&#8203;1229](https://redirect.github.com/rust-random/rand/issues/1229))
-   Add `UniformUsize` ([#&#8203;1487](https://redirect.github.com/rust-random/rand/issues/1487))
-   Remove support for generating `isize` and `usize` values with `StandardUniform`, `Uniform` (except via `UniformUsize`) and `Fill` and usage as a `WeightedAliasIndex` weight ([#&#8203;1487](https://redirect.github.com/rust-random/rand/issues/1487))
-   Add impl `DistString` for distributions `Slice<char>` and `Uniform<char>` ([#&#8203;1315](https://redirect.github.com/rust-random/rand/issues/1315))
-   Add fn `Slice::num_choices` ([#&#8203;1402](https://redirect.github.com/rust-random/rand/issues/1402))
-   Add fn `p()` for distribution `Bernoulli` to access probability ([#&#8203;1481](https://redirect.github.com/rust-random/rand/issues/1481))

##### API changes: Weighted distributions

-   Add `pub` module `rand::distr::weighted`, moving `WeightedIndex` there ([#&#8203;1548](https://redirect.github.com/rust-random/rand/issues/1548))
-   Add trait `weighted::Weight`, allowing `WeightedIndex` to trap overflow ([#&#8203;1353](https://redirect.github.com/rust-random/rand/issues/1353))
-   Add fns `weight, weights, total_weight` to distribution `WeightedIndex` ([#&#8203;1420](https://redirect.github.com/rust-random/rand/issues/1420))
-   Rename enum `WeightedError` to `weighted::Error`, revising variants ([#&#8203;1382](https://redirect.github.com/rust-random/rand/issues/1382)) and mark as `#[non_exhaustive]` ([#&#8203;1480](https://redirect.github.com/rust-random/rand/issues/1480))

##### API changes: SIMD

-   Switch to `std::simd`, expand SIMD & docs ([#&#8203;1239](https://redirect.github.com/rust-random/rand/issues/1239))

##### Reproducibility-breaking changes

-   Make `ReseedingRng::reseed` discard remaining data from the last block generated ([#&#8203;1379](https://redirect.github.com/rust-random/rand/issues/1379))
-   Change fn `SmallRng::seed_from_u64` implementation ([#&#8203;1203](https://redirect.github.com/rust-random/rand/issues/1203))
-   Allow `UniformFloat::new` samples and `UniformFloat::sample_single` to yield `high` ([#&#8203;1462](https://redirect.github.com/rust-random/rand/issues/1462))
-   Fix portability of distribution `Slice` ([#&#8203;1469](https://redirect.github.com/rust-random/rand/issues/1469))
-   Make `Uniform` for `usize` portable via `UniformUsize` ([#&#8203;1487](https://redirect.github.com/rust-random/rand/issues/1487))
-   Fix `IndexdRandom::choose_multiple_weighted` for very small seeds and optimize for large input length / low memory ([#&#8203;1530](https://redirect.github.com/rust-random/rand/issues/1530))

##### Reproducibility-breaking optimisations

-   Optimize fn `sample_floyd`, affecting output of `rand::seq::index::sample` and `rand::seq::SliceRandom::choose_multiple` ([#&#8203;1277](https://redirect.github.com/rust-random/rand/issues/1277))
-   New, faster algorithms for `IteratorRandom::choose` and `choose_stable` ([#&#8203;1268](https://redirect.github.com/rust-random/rand/issues/1268))
-   New, faster algorithms for `SliceRandom::shuffle` and `partial_shuffle` ([#&#8203;1272](https://redirect.github.com/rust-random/rand/issues/1272))
-   Optimize distribution `Uniform`: use Canon's method (single sampling) / Lemire's method (distribution sampling) for faster sampling (breaks value stability; [#&#8203;1287](https://redirect.github.com/rust-random/rand/issues/1287))
-   Optimize fn `sample_single_inclusive` for floats (+~20% perf) ([#&#8203;1289](https://redirect.github.com/rust-random/rand/issues/1289))

##### Other optimisations

-   Improve `SmallRng` initialization performance ([#&#8203;1482](https://redirect.github.com/rust-random/rand/issues/1482))
-   Optimise SIMD widening multiply ([#&#8203;1247](https://redirect.github.com/rust-random/rand/issues/1247))

##### Other

-   Add `Cargo.lock.msrv` file ([#&#8203;1275](https://redirect.github.com/rust-random/rand/issues/1275))
-   Reformat with `rustfmt` and enforce ([#&#8203;1448](https://redirect.github.com/rust-random/rand/issues/1448))
-   Apply Clippy suggestions and enforce ([#&#8203;1448](https://redirect.github.com/rust-random/rand/issues/1448), [#&#8203;1474](https://redirect.github.com/rust-random/rand/issues/1474))
-   Move all benchmarks to new `benches` crate ([#&#8203;1329](https://redirect.github.com/rust-random/rand/issues/1329), [#&#8203;1439](https://redirect.github.com/rust-random/rand/issues/1439)) and migrate to Criterion ([#&#8203;1490](https://redirect.github.com/rust-random/rand/issues/1490))

##### Documentation

-   Improve `ThreadRng` related docs ([#&#8203;1257](https://redirect.github.com/rust-random/rand/issues/1257))
-   Docs: enable experimental `--generate-link-to-definition` feature ([#&#8203;1327](https://redirect.github.com/rust-random/rand/issues/1327))
-   Better doc of crate features, use `doc_auto_cfg` ([#&#8203;1411](https://redirect.github.com/rust-random/rand/issues/1411), [#&#8203;1450](https://redirect.github.com/rust-random/rand/issues/1450))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xNDUuMCIsInVwZGF0ZWRJblZlciI6IjM5LjE0NS4wIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-02-03 00:25_

---

_Comment by @renovate[bot] on 2025-02-03 00:25_

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
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --package rand@0.8.5 --precise 0.9.0
    Updating crates.io index
error: failed to select a version for the requirement `rand = "^0.8"`
candidate versions found which didn't match: 0.9.0
location searched: crates.io index
required by package `quickcheck v1.0.3`
    ... which satisfies dependency `quickcheck = "^1.0.3"` (locked to 1.0.3) of package `red_knot_python_semantic v0.0.0 (/tmp/renovate/repos/github/astral-sh/ruff/crates/red_knot_python_semantic)`
    ... which satisfies path dependency `red_knot_python_semantic` (locked to 0.0.0) of package `red_knot v0.0.0 (/tmp/renovate/repos/github/astral-sh/ruff/crates/red_knot)`

```



---

_Comment by @github-actions[bot] on 2025-02-03 00:35_

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

_Assigned to @MichaReiser by @MichaReiser on 2025-02-03 10:12_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-02-03 10:15_

---

_Review requested from @carljm by @MichaReiser on 2025-02-03 11:11_

---

_Review requested from @MichaReiser by @MichaReiser on 2025-02-03 11:11_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-03 11:11_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-03 11:11_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-02-03 11:22_

---

_Merged by @MichaReiser on 2025-02-03 11:25_

---

_Closed by @MichaReiser on 2025-02-03 11:25_

---

_Branch deleted on 2025-02-03 11:25_

---
