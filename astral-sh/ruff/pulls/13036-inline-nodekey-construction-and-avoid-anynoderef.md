```yaml
number: 13036
title: "Inline NodeKey construction and avoid `AnyNodeRef`"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: perf-node-key
created_at: 2024-08-21T17:04:15Z
updated_at: 2024-08-21T17:14:03Z
url: https://github.com/astral-sh/ruff/pull/13036
synced_at: 2026-01-10T21:38:32Z
```

# Inline NodeKey construction and avoid `AnyNodeRef`

---

_Pull request opened by @MichaReiser on 2024-08-21 17:04_

_No description provided._

---

_Comment by @codspeed-hq[bot] on 2024-08-21 17:10_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/perf-node-key)

### Merging #13036 will **improve performances by 5.82%**

<sub>Comparing <code>perf-node-key</code> (82f33db) with <code>main</code> (dce87c2)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `perf-node-key` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 768.3 µs | 726 µs | +5.82% |


---

_Closed by @MichaReiser on 2024-08-21 17:14_

---
