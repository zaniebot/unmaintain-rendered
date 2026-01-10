```yaml
number: 9540
title: Avoid adding non-extra package with extra dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/transitive
created_at: 2024-11-30T14:31:04Z
updated_at: 2024-12-01T13:42:28Z
url: https://github.com/astral-sh/uv/pull/9540
synced_at: 2026-01-10T12:00:00Z
```

# Avoid adding non-extra package with extra dependencies

---

_Pull request opened by @charliermarsh on 2024-11-30 14:31_

## Summary

Previously, when we encountered `foo[bar]`, we'd add a dependency on `PubGrubPackage::Package` for `foo`, and then `PubGrubPackage::Extra` for `foo[bar]`.

Later, when we ask for the dependencies of the `PubGrubPackage::Extra`, we add `PubGrubPackage::Package` for `foo`, and `PubGrubPackage::Package` for `foo[bar]`. This is an intentional strategy because it ensures that PubGrub "knows" that these have to be solved to the same version as early as possible.

It turns out that the first part here ("add a dependency on `PubGrubPackage::Package` for `foo`") is suboptimal, because it means PubGrub might try to solve _just_ `foo` without realizing that it also has to accommodate all the constraints from the extra.

Instead, we now add _just_ `PubGrubPackage::Extra` for `foo[bar]`, and defer adding the base package. It looks like this leads to a far more efficient solve for Airflow.

---

_Comment by @codspeed-hq[bot] on 2024-11-30 14:37_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Ftransitive)

### Merging #9540 will **improve performances by 77.1%**

<sub>Comparing <code>charlie/transitive</code> (042f88a) with <code>main</code> (8126a5e)</sub>



### Summary

`⚡ 1` improvements  
`✅ 13` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/transitive` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `resolve_warm_airflow` | 2 s | 1.1 s | +77.1% |


---

_Comment by @charliermarsh on 2024-11-30 14:53_

The gain actually seems real? At least to some degree:

```
❯ hyperfine "./target/release/uv pip compile req.in" "./target/release/transitive pip compile req.in" --warmup 10 --runs 50
Benchmark 1: ./target/release/uv pip compile req.in
  Time (mean ± σ):     198.6 ms ±   6.0 ms    [User: 224.9 ms, System: 244.6 ms]
  Range (min … max):   189.8 ms … 213.9 ms    50 runs

Benchmark 2: ./target/release/transitive pip compile req.in
  Time (mean ± σ):     149.4 ms ±   4.1 ms    [User: 160.6 ms, System: 182.2 ms]
  Range (min … max):   144.9 ms … 173.2 ms    50 runs

Summary
  ./target/release/transitive pip compile req.in ran
    1.33 ± 0.05 times faster than ./target/release/uv pip compile req.in
```

---

_Comment by @charliermarsh on 2024-11-30 14:54_

This change is sound but I clearly need to modify some of how we interpret the downstream graph in `pip compile`.

---

_Marked ready for review by @charliermarsh on 2024-11-30 22:18_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-30 22:19_

---

_Review requested from @konstin by @charliermarsh on 2024-11-30 22:19_

---

_Label `performance` added by @charliermarsh on 2024-11-30 22:19_

---

_@charliermarsh reviewed on 2024-11-30 22:33_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2475 on 2024-11-30 22:33_

I reasoned through these and most of them can be removed.

---

_@charliermarsh reviewed on 2024-11-30 22:33_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2347 on 2024-11-30 22:33_

If we just use `name`, we can rely on `warn_user_once`.

---

_@konstin approved on 2024-12-01 13:29_

wow wouldn't have thought about that trick for such a speed!

---

_Merged by @charliermarsh on 2024-12-01 13:42_

---

_Closed by @charliermarsh on 2024-12-01 13:42_

---

_Branch deleted on 2024-12-01 13:42_

---
