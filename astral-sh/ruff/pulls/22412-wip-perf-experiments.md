```yaml
number: 22412
title: "[WIP] perf experiments"
type: pull_request
state: open
author: dylwil3
labels: []
assignees: []
draft: true
base: main
head: micro-opt
created_at: 2026-01-06T05:26:09Z
updated_at: 2026-01-09T23:48:14Z
url: https://github.com/astral-sh/ruff/pull/22412
synced_at: 2026-01-12T15:57:49Z
```

# [WIP] perf experiments

---

_@dylwil3_

Don't mind me...


---

_Comment by @astral-sh-bot[bot] on 2026-01-06 05:34_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @codspeed-hq[bot] on 2026-01-06 05:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dylwil3%3Amicro-opt?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **improve performance by 5.29%**

<sub>Comparing <code>dylwil3:micro-opt</code> (56402b0) with <code>main</code> (2c7ac17)</sub>



### Summary

`⚡ 1` improved benchmark  
`✅ 52` untouched benchmarks  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | Simulation | [`` linter/default-rules[pydantic/types.py] ``](https://codspeed.io/astral-sh/ruff/branches/dylwil3%3Amicro-opt?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Adefault_rules%3A%3Abenchmark_default_rules%3A%3Alinter%2Fdefault-rules%5Bpydantic%2Ftypes.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.2 ms | 2.1 ms | +5.29% |


---
