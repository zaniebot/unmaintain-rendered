```yaml
number: 22020
title: "[ty] Experiment"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: main
head: micha/has_relation_to
created_at: 2025-12-17T11:25:17Z
updated_at: 2025-12-18T17:58:08Z
url: https://github.com/astral-sh/ruff/pull/22020
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Experiment

---

_@MichaReiser_

Just an experiment, entirely written by claude. I didn't review the code. There's at least one test failure

---

_Label `ty` added by @MichaReiser on 2025-12-17 11:25_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 11:27_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @MichaReiser on 2025-12-17 11:45_

Well, what claude did here is certainly useless 😆 

---

_Comment by @codspeed-hq[bot] on 2025-12-17 11:46_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fhas_relation_to?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22020 will **degrade performances by 16.86%**

<sub>Comparing <code>micha/has_relation_to</code> (39b5c69) with <code>main</code> (85af715)</sub>



### Summary

`⚡ 3` improvements  
`❌ 3` regressions  
`✅ 16` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fhas_relation_to?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fhas_relation_to?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 216.2 s | 260 s | -16.86% |
| ⚡ | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fhas_relation_to?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 20.2 s | 18.4 s | +9.93% |
| ⚡ | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fhas_relation_to?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 66.4 s | 61 s | +8.94% |
| ⚡ | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fhas_relation_to?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 106 s | 44.1 s | ×2.4 |
| ❌ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fhas_relation_to?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.3 s | 1.4 s | -4.03% |
| ❌ | Simulation | [`` ty_micro[many_enum_members] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fhas_relation_to?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_enum_members%3A%3Aty_micro%5Bmany_enum_members%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 123.4 ms | 129.1 ms | -4.45% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/micha%2Fhas_relation_to?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @MichaReiser on 2025-12-17 11:48_

What claude did was caching `has_relation_to` in addition to `is_redudant_to`. This does show very promising results on `colour-science` (2x improvement) but it also significantely increases memory usage (we would want within revision GC)

---

_Comment by @AlexWaygood on 2025-12-17 11:50_

> This does show very promising results on `colour-science` (2x improvement) but it also significantely increases memory usage (we would want within revision GC)

it also shows us getting significantly worse on some of the codebases where we already have our most pathological performance (pydantic). And there are new panics in the mypy_primer CI job.

---

_Comment by @MichaReiser on 2025-12-17 11:53_

> it also shows us getting significantly worse on some of the codebases where we already have our most pathological performance (pydantic). And there are new panics in the mypy_primer CI job.

Oh I know. I'm not saying we should do this or this is in any way a good PR (see summary)

---

_Closed by @MichaReiser on 2025-12-18 17:58_

---
