```yaml
number: 14240
title: Remove wheel filename benchmark
type: pull_request
state: merged
author: charliermarsh
labels:
  - ci-flake
assignees: []
merged: true
base: main
head: charlie/be
created_at: 2025-06-24T13:44:19Z
updated_at: 2025-06-24T15:54:14Z
url: https://github.com/astral-sh/uv/pull/14240
synced_at: 2026-01-12T16:11:06Z
```

# Remove wheel filename benchmark

---

_@charliermarsh_

## Summary

This flakes often and we don't really need it to be monitored continuously. We can always revive it from Git later.

Closes https://github.com/astral-sh/uv/issues/13952.


---

_Review requested from @Gankra by @charliermarsh on 2025-06-24 13:44_

---

_Review requested from @konstin by @charliermarsh on 2025-06-24 13:44_

---

_Label `ci-flake` added by @charliermarsh on 2025-06-24 13:44_

---

_Comment by @codspeed-hq[bot] on 2025-06-24 13:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fbe?runnerMode=Instrumentation)

### Merging #14240 will **not alter performance**

<sub>Comparing <code>charlie/be</code> (3ddca4d) with <code>main</code> (19c58c7)</sub>



### Summary

`✅ 3` untouched benchmarks  
`⁉️ 9` dropped benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie%2Fbe?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⁉️ | `` build_platform_tags[burntsushi-archlinux] `` | 278.9 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing[flyte-long-compatible] `` | 13.2 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing[flyte-long-incompatible] `` | 15.4 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing[flyte-short-compatible] `` | 4.9 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing_failure[flyte-long-extension] `` | 1.7 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing_failure[flyte-short-extension] `` | 1.7 µs | N/A | N/A |
| ⁉️ | `` wheelname_tag_compatibility[flyte-long-compatible] `` | 1.9 µs | N/A | N/A |
| ⁉️ | `` wheelname_tag_compatibility[flyte-long-incompatible] `` | 1.3 µs | N/A | N/A |
| ⁉️ | `` wheelname_tag_compatibility[flyte-short-compatible] `` | 1.6 µs | N/A | N/A |


---

_Comment by @codspeed-hq[bot] on 2025-06-24 13:56_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fbe?runnerMode=WallTime)

### Merging #14240 will **not alter performance**

<sub>Comparing <code>charlie/be</code> (3ddca4d) with <code>main</code> (19c58c7)</sub>



### Summary

`✅ 3` untouched benchmarks  
`⁉️ 9` dropped benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie%2Fbe?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⁉️ | `` build_platform_tags[burntsushi-archlinux] `` | 161.2 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing[flyte-long-compatible] `` | 1.3 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing[flyte-long-incompatible] `` | 2.1 µs | N/A | N/A |
| ⁉️ | `` wheelname_parsing[flyte-short-compatible] `` | 298 ns | N/A | N/A |
| ⁉️ | `` wheelname_parsing_failure[flyte-long-extension] `` | 96 ns | N/A | N/A |
| ⁉️ | `` wheelname_parsing_failure[flyte-short-extension] `` | 92 ns | N/A | N/A |
| ⁉️ | `` wheelname_tag_compatibility[flyte-long-compatible] `` | 90 ns | N/A | N/A |
| ⁉️ | `` wheelname_tag_compatibility[flyte-long-incompatible] `` | 59 ns | N/A | N/A |
| ⁉️ | `` wheelname_tag_compatibility[flyte-short-compatible] `` | 68 ns | N/A | N/A |


---

_Merged by @charliermarsh on 2025-06-24 15:54_

---

_Closed by @charliermarsh on 2025-06-24 15:54_

---

_Branch deleted on 2025-06-24 15:54_

---
