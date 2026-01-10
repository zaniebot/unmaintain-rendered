```yaml
number: 5959
title: Use a larger runner for benchmark job
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/benchmark-bigger-runner
created_at: 2024-08-09T14:43:06Z
updated_at: 2024-08-09T16:02:30Z
url: https://github.com/astral-sh/uv/pull/5959
synced_at: 2026-01-10T13:31:54Z
```

# Use a larger runner for benchmark job

---

_Pull request opened by @zanieb on 2024-08-09 14:43_

May not have an effect, testing

---

_Comment by @codspeed-hq[bot] on 2024-08-09 14:59_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb/benchmark-bigger-runner)

### Merging #5959 will **not alter performance**

<sub>Comparing <code>zb/benchmark-bigger-runner</code> (1220779) with <code>main</code> (4330f97)</sub>



### Summary

`✅ 11` untouched benchmarks

`🆕 4` new benchmarks
`⁉️ 2` dropped benchmarks


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/zb/benchmark-bigger-runner)._

### Benchmarks breakdown

|     | Benchmark | `main` | `zb/benchmark-bigger-runner` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 🆕 | `resolve_warm_airflow` | N/A | 2 s | N/A |
| 🆕 | `resolve_warm_airflow_universal` | N/A | 7.7 s | N/A |
| 🆕 | `resolve_warm_jupyter` | N/A | 85.1 ms | N/A |
| 🆕 | `resolve_warm_jupyter_universal` | N/A | 271.1 ms | N/A |
| ⁉️ | `resolve_warm_airflow` | 1 s | N/A | N/A |
| ⁉️ | `resolve_warm_jupyter` | 94.4 ms | N/A | N/A |


---

_Comment by @zanieb on 2024-08-09 16:02_

No difference.

---

_Closed by @zanieb on 2024-08-09 16:02_

---
