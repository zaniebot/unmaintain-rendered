```yaml
number: 15878
title: Vendor benchmark test files
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/remove-download-functionality-from-bench
created_at: 2025-02-02T16:10:42Z
updated_at: 2025-02-02T18:19:37Z
url: https://github.com/astral-sh/ruff/pull/15878
synced_at: 2026-01-12T15:55:53Z
```

# Vendor benchmark test files

---

_@MichaReiser_

## Summary
This PR vendors the benchmark test files. This has the advantage
that we can remove the `ureq` dependency, running the benchmarks for
the first time no longer requires an internet connectivity, and 
it simplifies the benchmark scripts. 

This closes https://github.com/astral-sh/ruff/pull/15759

## Test Plan

`cargo bench -p ruff_benchmark`


---

_Label `internal` added by @MichaReiser on 2025-02-02 16:10_

---

_Comment by @codspeed-hq[bot] on 2025-02-02 16:17_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fremove-download-functionality-from-bench)

### Merging #15878 will **improve performances by 4.45%**

<sub>Comparing <code>micha/remove-download-functionality-from-bench</code> (1136b75) with <code>main</code> (d9a1034)</sub>



### Summary

`⚡ 1` improvements  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` lexer[numpy/globals.py] `` | 30.5 µs | 29.2 µs | +4.45% |


---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/resources/README.md`:1 on 2025-02-02 16:22_

```suggestion
This directory vendors some files from actual projects.
```

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/resources/README.md`:2 on 2025-02-02 16:22_

```suggestion
This is to benchmark Ruff's performance against real-world
```

---

_@AlexWaygood approved on 2025-02-02 16:23_

(haven't done a line-by-line review, but I approve of the concept!)

---

_Comment by @github-actions[bot] on 2025-02-02 16:28_

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

_Merged by @MichaReiser on 2025-02-02 18:16_

---

_Closed by @MichaReiser on 2025-02-02 18:16_

---

_Branch deleted on 2025-02-02 18:16_

---
