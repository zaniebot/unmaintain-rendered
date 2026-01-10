```yaml
number: 10350
title: Avoid overcounting versions in batch prefetcher
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/opt-batch-prefetch
created_at: 2025-01-07T09:42:23Z
updated_at: 2025-01-07T14:12:19Z
url: https://github.com/astral-sh/uv/pull/10350
synced_at: 2026-01-10T11:44:44Z
```

# Avoid overcounting versions in batch prefetcher

---

_Pull request opened by @konstin on 2025-01-07 09:42_

Ref https://github.com/astral-sh/uv/issues/10344

The batch prefetcher used to be over-eager in prefetching due to repeatedly trying the same version counting each time towards the batch prefetch threshold. By tracking the actual versions in a hashset, we avoid stalling the resolver thread for expensive rkyv deserialization for prefetching for version that we won't need.

---

_Label `performance` added by @konstin on 2025-01-07 09:42_

---

_Comment by @codspeed-hq[bot] on 2025-01-07 09:49_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti%2Fopt-batch-prefetch)

### Merging #10350 will **improve performances by 19.1%**

<sub>Comparing <code>konsti/opt-batch-prefetch</code> (2edee7b) with <code>main</code> (c6ac121)</sub>



### Summary

`⚡ 1` improvements  
`✅ 13` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `main` | `konsti/opt-batch-prefetch` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` resolve_warm_airflow `` | 1.4 s | 1.1 s | +19.1% |


---

_@charliermarsh approved on 2025-01-07 13:52_

---

_Merged by @konstin on 2025-01-07 14:12_

---

_Closed by @konstin on 2025-01-07 14:12_

---

_Branch deleted on 2025-01-07 14:12_

---
