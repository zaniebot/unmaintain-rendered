```yaml
number: 16887
title: "Bump `astral-tl` to v0.7.10"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/tl
created_at: 2025-11-28T19:36:09Z
updated_at: 2025-11-28T20:02:45Z
url: https://github.com/astral-sh/uv/pull/16887
synced_at: 2026-01-12T16:12:30Z
```

# Bump `astral-tl` to v0.7.10

---

_@charliermarsh_

## Summary

Enables SIMD for HTML parsing.

---

_Label `performance` added by @charliermarsh on 2025-11-28 19:36_

---

_Marked ready for review by @charliermarsh on 2025-11-28 19:36_

---

_Merged by @charliermarsh on 2025-11-28 19:49_

---

_Closed by @charliermarsh on 2025-11-28 19:49_

---

_Branch deleted on 2025-11-28 19:49_

---

_Comment by @codspeed-hq[bot] on 2025-11-28 19:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Ftl?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16887 will **degrade performances by 12.84%**

<sub>Comparing <code>charlie/tl</code> (f2c33b4) with <code>main</code> (5498e4d)</sub>



### Summary

`❌ 1` regression  
`✅ 5` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie%2Ftl?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` resolve_warm_airflow ``](https://codspeed.io/astral-sh/uv/branches/charlie%2Ftl?uri=crates%2Fuv-bench%2Fbenches%2Fuv.rs%3A%3Auv%3A%3Aresolve_warm_airflow%3A%3Aresolve_warm_airflow&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 779.8 ms | 894.7 ms | -12.84% |


---
