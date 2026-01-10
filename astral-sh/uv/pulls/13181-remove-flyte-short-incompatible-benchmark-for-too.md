```yaml
number: 13181
title: Remove flyte-short-incompatible benchmark for too many false positives
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/remove-flyte-short-incompatible
created_at: 2025-04-28T15:37:46Z
updated_at: 2025-04-28T16:01:36Z
url: https://github.com/astral-sh/uv/pull/13181
synced_at: 2026-01-10T11:10:41Z
```

# Remove flyte-short-incompatible benchmark for too many false positives

---

_Pull request opened by @konstin on 2025-04-28 15:37_

The benchmark has recurring false positives (https://github.com/astral-sh/uv/issues?q=flyte-short-incompatible%20), so we're removing it.

---

_Label `internal` added by @konstin on 2025-04-28 15:37_

---

_Review requested from @BurntSushi by @konstin on 2025-04-28 15:37_

---

_Review requested from @charliermarsh by @konstin on 2025-04-28 15:37_

---

_Comment by @codspeed-hq[bot] on 2025-04-28 15:45_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti%2Fremove-flyte-short-incompatible)

### Merging #13181 will **not alter performance**

<sub>Comparing <code>konsti/remove-flyte-short-incompatible</code> (4b2daa9) with <code>main</code> (bb0158d)</sub>



### Summary

`✅ 12` untouched benchmarks  
`⁉️ 2 (👁 2)` dropped benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` wheelname_parsing[flyte-short-incompatible] `` | 4.9 µs | N/A | N/A |
| 👁 | `` wheelname_tag_compatibility[flyte-short-incompatible] `` | 829.7 ns | N/A | N/A |


---

_@BurntSushi approved on 2025-04-28 15:51_

---

_Merged by @konstin on 2025-04-28 16:01_

---

_Closed by @konstin on 2025-04-28 16:01_

---

_Branch deleted on 2025-04-28 16:01_

---
