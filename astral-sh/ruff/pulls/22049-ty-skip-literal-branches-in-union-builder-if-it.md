```yaml
number: 22049
title: "[ty] Skip literal branches in union builder if it contains no literals"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: micha/union-builder-simplify
head: micha/union-builder-literal
created_at: 2025-12-18T08:33:39Z
updated_at: 2025-12-18T09:01:03Z
url: https://github.com/astral-sh/ruff/pull/22049
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Skip literal branches in union builder if it contains no literals

---

_Pull request opened by @MichaReiser on 2025-12-18 08:33_

_No description provided._

---

_Label `ty` added by @MichaReiser on 2025-12-18 08:33_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 08:35_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @codspeed-hq[bot] on 2025-12-18 09:00_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-literal?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22049 will **degrade performances by 59.81%**

<sub>Comparing <code>micha/union-builder-literal</code> (2f6b96f) with <code>micha/union-builder-simplify</code> (eecc160)</sub>



### Summary

`❌ 6` regressions  
`✅ 16` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-literal?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 62.3 s | 79.1 s | -21.27% |
| ❌ | WallTime | [`` multithreaded[altair] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amultithreaded%5Baltair%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.4 s | 3.4 s | -59.81% |
| ❌ | WallTime | [`` small[altair] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Baltair%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 5.2 s | 7.9 s | -34.98% |
| ❌ | WallTime | [`` small[freqtrade] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bfreqtrade%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.8 s | 9.4 s | -16.7% |
| ❌ | WallTime | [`` large[sympy] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bsympy%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 49.4 s | 83.5 s | -40.89% |
| ❌ | Simulation | [`` anyio ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aanyio%3A%3Aproject%3A%3Aanyio&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.3 s | 1.5 s | -8.73% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-literal?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @MichaReiser on 2025-12-18 09:01_

Haha, that's not the result I expected. I must be overlooking something why it is that we always want to iterate over `elements` even if there are no existing literals in the union

---

_Closed by @MichaReiser on 2025-12-18 09:01_

---
