```yaml
number: 16834
title: "Disable `test_simultaneous_multiple_create_delete_single_thread` on Windows"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/test-flake-ii
created_at: 2025-11-24T14:57:23Z
updated_at: 2025-11-24T15:16:28Z
url: https://github.com/astral-sh/uv/pull/16834
synced_at: 2026-01-12T16:12:28Z
```

# Disable `test_simultaneous_multiple_create_delete_single_thread` on Windows

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/16096

---

_Merged by @zanieb on 2025-11-24 15:10_

---

_Closed by @zanieb on 2025-11-24 15:10_

---

_Branch deleted on 2025-11-24 15:10_

---

_Comment by @codspeed-hq[bot] on 2025-11-24 15:16_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb%2Ftest-flake-ii?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16834 will **degrade performances by 13.01%**

<sub>Comparing <code>zb/test-flake-ii</code> (983bf92) with <code>main</code> (d6eb285)</sub>



### Summary

`❌ 1` regression  
`✅ 5` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/zb%2Ftest-flake-ii?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` resolve_warm_airflow ``](https://codspeed.io/astral-sh/uv/branches/zb%2Ftest-flake-ii?uri=crates%2Fuv-bench%2Fbenches%2Fuv.rs%3A%3Auv%3A%3Aresolve_warm_airflow%3A%3Aresolve_warm_airflow&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 808.3 ms | 929.3 ms | -13.01% |


---
