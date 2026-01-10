```yaml
number: 7226
title: Avoid batch prefetching for un-optimized registries
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/range-stop
created_at: 2024-09-09T18:17:53Z
updated_at: 2024-09-09T19:46:20Z
url: https://github.com/astral-sh/uv/pull/7226
synced_at: 2026-01-10T12:53:42Z
```

# Avoid batch prefetching for un-optimized registries

---

_Pull request opened by @charliermarsh on 2024-09-09 18:17_

## Summary

We now track the discovered `IndexCapabilities` for each `IndexUrl`. If we learn that an index doesn't support range requests, we avoid doing any batch prefetching.

Closes https://github.com/astral-sh/uv/issues/7221.


---

_Label `performance` added by @charliermarsh on 2024-09-09 18:19_

---

_Review requested from @konstin by @charliermarsh on 2024-09-09 18:19_

---

_Comment by @codspeed-hq[bot] on 2024-09-09 18:32_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/range-stop)

### Merging #7226 will **degrade performances by 11.51%**

<sub>Comparing <code>charlie/range-stop</code> (b322350) with <code>main</code> (970bd1a)</sub>



### Summary

`❌ 1` regressions
`✅ 13` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie/range-stop)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/range-stop` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 897.2 ns | 1,013.9 ns | -11.51% |


---

_@konstin approved on 2024-09-09 18:40_

---

_Merged by @charliermarsh on 2024-09-09 19:46_

---

_Closed by @charliermarsh on 2024-09-09 19:46_

---

_Branch deleted on 2024-09-09 19:46_

---
