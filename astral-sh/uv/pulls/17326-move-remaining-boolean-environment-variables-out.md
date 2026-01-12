```yaml
number: 17326
title: Move remaining Boolean environment variables out of Clap
type: pull_request
state: open
author: charliermarsh
labels:
  - internal
assignees: []
draft: true
base: main
head: charlie/env-everywhere
created_at: 2026-01-05T18:49:48Z
updated_at: 2026-01-06T20:43:06Z
url: https://github.com/astral-sh/uv/pull/17326
synced_at: 2026-01-12T16:12:43Z
```

# Move remaining Boolean environment variables out of Clap

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2026-01-05 18:50_

---

_Renamed from "Move remaining environment variable handling out of Clap" to "Move remaining Boolean environment variables out of Clap" by @charliermarsh on 2026-01-05 18:56_

---

_Comment by @codspeed-hq[bot] on 2026-01-06 18:04_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fenv-everywhere?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **improve performance by 12.01%**

<sub>Comparing <code>charlie/env-everywhere</code> (6a0b84c) with <code>charlie/err-from-env</code> (4642228)</sub>



### Summary

`⚡ 1` improved benchmark  
`✅ 4` untouched benchmarks  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | Simulation | [`` resolve_warm_jupyter ``](https://codspeed.io/astral-sh/uv/branches/charlie%2Fenv-everywhere?uri=crates%2Fuv-bench%2Fbenches%2Fuv.rs%3A%3Auv%3A%3Aresolve_warm_jupyter%3A%3Aresolve_warm_jupyter&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 83.3 ms | 74.4 ms | +12.01% |


---
