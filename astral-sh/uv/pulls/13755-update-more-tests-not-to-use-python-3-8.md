```yaml
number: 13755
title: Update more tests not to use Python 3.8
type: pull_request
state: merged
author: mgorny
labels: []
assignees: []
merged: true
base: main
head: more-py38
created_at: 2025-05-31T15:49:57Z
updated_at: 2025-06-02T07:40:18Z
url: https://github.com/astral-sh/uv/pull/13755
synced_at: 2026-01-12T16:10:50Z
```

# Update more tests not to use Python 3.8

---

_@mgorny_

## Summary

Update a few more tests to avoid using Python 3.8 unnecessarily. From what I can see, neither of these really need that specific Python version, so just update them to use 3.9 instead.

Bug #13676

## Test Plan

Uninstall Python 3.8 and run: `cargo test --no-default-features --features git,pypi,python`

One test failure remains.

---

_Comment by @codspeed-hq[bot] on 2025-05-31 16:04_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/mgorny%3Amore-py38)

### Merging #13755 will **improve performances by 15.79%**

<sub>Comparing <code>mgorny:more-py38</code> (d45b36b) with <code>main</code> (13a86a2)</sub>



### Summary

`⚡ 1` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing_failure[flyte-long-extension] `` | 110 ns | 95 ns | +15.79% |


---

_@konstin approved on 2025-06-02 07:22_

---

_Merged by @konstin on 2025-06-02 07:22_

---

_Closed by @konstin on 2025-06-02 07:22_

---

_Branch deleted on 2025-06-02 07:40_

---

_Comment by @mgorny on 2025-06-02 07:40_

Thanks!

---
