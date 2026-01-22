```yaml
number: 22794
title: "[ty] loop control flow analysis using loop header definitions"
type: pull_request
state: open
author: oconnor663
labels: []
assignees: []
draft: true
base: main
head: jack/cyclic_control_flow
created_at: 2026-01-22T03:40:09Z
updated_at: 2026-01-22T03:52:28Z
url: https://github.com/astral-sh/ruff/pull/22794
synced_at: 2026-01-22T04:09:27Z
```

# [ty] loop control flow analysis using loop header definitions

---

_@oconnor663_

This is a draft PR to trigger CI and an ecosystem report. I still need to "own" some of Claude's code here, but all tests are plausibly passing. I expect the ecosystem report will have a lot of findings. I'm also not especially optimistic about the CodSpeed numbers. (This PR adds an extra preliminary AST pass to every loop body.)

---

_Comment by @astral-sh-bot[bot] on 2026-01-22 03:41_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @codspeed-hq[bot] on 2026-01-22 03:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jack%2Fcyclic_control_flow?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 9.1%**

<sub>Comparing <code>jack/cyclic_control_flow</code> (763eb70) with <code>main</code> (c5b4ee6)</sub>



### Summary

`❌ 3` regressed benchmarks  
`✅ 20` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/jack%2Fcyclic_control_flow?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` sympy ``](https://codspeed.io/astral-sh/ruff/branches/jack%2Fcyclic_control_flow?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asympy&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 50.5 s | 55.5 s | -9.1% |
| ❌ | Simulation | [`` ty_check_file[cold] ``](https://codspeed.io/astral-sh/ruff/branches/jack%2Fcyclic_control_flow?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_cold%3A%3Aty_check_file%5Bcold%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 130.7 ms | 136.4 ms | -4.13% |
| ❌ | Simulation | [`` ty_check_file[incremental] ``](https://codspeed.io/astral-sh/ruff/branches/jack%2Fcyclic_control_flow?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_incremental%3A%3Aty_check_file%5Bincremental%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 6.1 ms | 6.5 ms | -4.94% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/jack%2Fcyclic_control_flow?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---
