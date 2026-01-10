```yaml
number: 12091
title: Prioritize unspecified over conflict late
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
draft: true
base: main
head: konsti/prio-late-after-unspecified
created_at: 2025-03-10T10:05:08Z
updated_at: 2025-03-10T23:05:31Z
url: https://github.com/astral-sh/uv/pull/12091
synced_at: 2026-01-10T11:10:39Z
```

# Prioritize unspecified over conflict late

---

_Pull request opened by @konstin on 2025-03-10 10:05_

For #12060

---

_Comment by @codspeed-hq[bot] on 2025-03-10 10:12_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti%2Fprio-late-after-unspecified)

### Merging #12091 will **improve performances by 23.66%**

<sub>Comparing <code>konsti/prio-late-after-unspecified</code> (facd82d) with <code>main</code> (9776dc5)</sub>



### Summary

`⚡ 1` improvements  
`✅ 13` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` resolve_warm_airflow `` | 688 ms | 556.4 ms | +23.66% |


---

_Comment by @konstin on 2025-03-10 23:05_

We can still do it, but it needs some stronger support and ecosystem testing to avoid regressing other use cases by fixing one specific case.

---

_Closed by @konstin on 2025-03-10 23:05_

---
