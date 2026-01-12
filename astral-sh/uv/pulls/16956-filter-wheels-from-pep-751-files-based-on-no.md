```yaml
number: 16956
title: "Filter wheels from PEP 751 files based on `--no-binary` et al in `uv pip compile`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/foo
created_at: 2025-12-03T06:15:10Z
updated_at: 2025-12-03T12:51:37Z
url: https://github.com/astral-sh/uv/pull/16956
synced_at: 2026-01-12T16:12:32Z
```

# Filter wheels from PEP 751 files based on `--no-binary` et al in `uv pip compile`

---

_@charliermarsh_

## Summary

Like in `uv.lock`, we should omit artifacts that are filtered out by `--no-binary` or by the target platform tags.

Closes https://github.com/astral-sh/uv/issues/13413.


---

_Review requested from @zanieb by @charliermarsh on 2025-12-03 06:15_

---

_Review requested from @konstin by @charliermarsh on 2025-12-03 06:15_

---

_Label `enhancement` added by @charliermarsh on 2025-12-03 06:15_

---

_Marked ready for review by @charliermarsh on 2025-12-03 06:15_

---

_Comment by @codspeed-hq[bot] on 2025-12-03 06:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Ffoo?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16956 will **degrade performances by 12.37%**

<sub>Comparing <code>charlie/foo</code> (17127fb) with <code>main</code> (d2db069)</sub>



### Summary

`❌ 1` regression  
`✅ 5` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie%2Ffoo?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` resolve_warm_airflow ``](https://codspeed.io/astral-sh/uv/branches/charlie%2Ffoo?uri=crates%2Fuv-bench%2Fbenches%2Fuv.rs%3A%3Auv%3A%3Aresolve_warm_airflow%3A%3Aresolve_warm_airflow&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 780.5 ms | 890.7 ms | -12.37% |


---

_@konstin approved on 2025-12-03 09:01_

---

_@zanieb approved on 2025-12-03 12:51_

---

_Merged by @zanieb on 2025-12-03 12:51_

---

_Closed by @zanieb on 2025-12-03 12:51_

---

_Branch deleted on 2025-12-03 12:51_

---
