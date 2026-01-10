```yaml
number: 21261
title: "Run codspeed benchmarks with `profiling` profile"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - ci
assignees: []
merged: true
base: main
head: ibraheem/codspeed-profile
created_at: 2025-11-03T18:37:38Z
updated_at: 2025-11-04T14:59:42Z
url: https://github.com/astral-sh/ruff/pull/21261
synced_at: 2026-01-10T16:53:55Z
```

# Run codspeed benchmarks with `profiling` profile

---

_Pull request opened by @ibraheemdev on 2025-11-03 18:37_

## Summary

The codspeed action currently takes >15m to run. Let's see how much this improves it..

---

_Label `internal` added by @ibraheemdev on 2025-11-03 18:37_

---

_Label `ci` added by @ibraheemdev on 2025-11-03 18:37_

---

_Comment by @codspeed-hq[bot] on 2025-11-03 18:55_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21261 will **degrade performances by 20.06%**

<sub>Comparing <code>ibraheem/codspeed-profile</code> (ec0a2d2) with <code>main</code> (fe4ee81)</sub>



### Summary

`❌ 39` regressions  
`✅ 13` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` formatter[numpy/globals.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Fformatter.rs%3A%3Aformatter%3A%3Abenchmark_formatter%3A%3Aformatter%5Bnumpy%2Fglobals.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 234.9 µs | 246.1 µs | -4.56% |
| ❌ | Simulation | [`` formatter[unicode/pypinyin.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Fformatter.rs%3A%3Aformatter%3A%3Abenchmark_formatter%3A%3Aformatter%5Bunicode%2Fpypinyin.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 647.3 µs | 675.4 µs | -4.16% |
| ❌ | Simulation | [`` lexer[large/dataset.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flexer.rs%3A%3Alexer%3A%3Abenchmark_lexer%3A%3Alexer%5Blarge%2Fdataset.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.1 ms | 1.1 ms | -5.14% |
| ❌ | Simulation | [`` linter/all-rules[large/dataset.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Aall_rules%3A%3Abenchmark_all_rules%3A%3Alinter%2Fall-rules%5Blarge%2Fdataset.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 16.2 ms | 18.6 ms | -12.76% |
| ❌ | Simulation | [`` linter/all-rules[numpy/ctypeslib.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Aall_rules%3A%3Abenchmark_all_rules%3A%3Alinter%2Fall-rules%5Bnumpy%2Fctypeslib.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 3.9 ms | 4.3 ms | -10.62% |
| ❌ | Simulation | [`` linter/all-rules[numpy/globals.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Aall_rules%3A%3Abenchmark_all_rules%3A%3Alinter%2Fall-rules%5Bnumpy%2Fglobals.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 648 µs | 732.3 µs | -11.51% |
| ❌ | Simulation | [`` linter/all-rules[pydantic/types.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Aall_rules%3A%3Abenchmark_all_rules%3A%3Alinter%2Fall-rules%5Bpydantic%2Ftypes.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.5 ms | 8.5 ms | -11.76% |
| ❌ | Simulation | [`` linter/all-rules[unicode/pypinyin.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Aall_rules%3A%3Abenchmark_all_rules%3A%3Alinter%2Fall-rules%5Bunicode%2Fpypinyin.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.7 ms | 1.9 ms | -12.65% |
| ❌ | Simulation | [`` linter/default-rules[numpy/ctypeslib.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Adefault_rules%3A%3Abenchmark_default_rules%3A%3Alinter%2Fdefault-rules%5Bnumpy%2Fctypeslib.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 984.7 µs | 1,032.7 µs | -4.65% |
| ❌ | Simulation | [`` linter/default-rules[pydantic/types.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Adefault_rules%3A%3Abenchmark_default_rules%3A%3Alinter%2Fdefault-rules%5Bpydantic%2Ftypes.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.1 ms | 2.2 ms | -5.86% |
| ❌ | Simulation | [`` linter/default-rules[unicode/pypinyin.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Adefault_rules%3A%3Abenchmark_default_rules%3A%3Alinter%2Fdefault-rules%5Bunicode%2Fpypinyin.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 390.4 µs | 410 µs | -4.78% |
| ❌ | Simulation | [`` linter/all-with-preview-rules[large/dataset.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Blarge%2Fdataset.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 19.6 ms | 22.3 ms | -12.37% |
| ❌ | Simulation | [`` linter/all-with-preview-rules[numpy/ctypeslib.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bnumpy%2Fctypeslib.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 4.6 ms | 5.1 ms | -10.62% |
| ❌ | Simulation | [`` linter/all-with-preview-rules[numpy/globals.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bnumpy%2Fglobals.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 738.7 µs | 833.4 µs | -11.36% |
| ❌ | Simulation | [`` linter/all-with-preview-rules[pydantic/types.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bpydantic%2Ftypes.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 8.9 ms | 10.1 ms | -11.32% |
| ❌ | Simulation | [`` linter/all-with-preview-rules[unicode/pypinyin.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bunicode%2Fpypinyin.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.9 ms | 2.2 ms | -12.56% |
| ❌ | Simulation | [`` parser[large/dataset.py] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Fparser.rs%3A%3Aparser%3A%3Abenchmark_parser%3A%3Aparser%5Blarge%2Fdataset.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 4.9 ms | 5.1 ms | -4.42% |
| ❌ | Simulation | [`` ty_check_file[cold] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_cold%3A%3Aty_check_file%5Bcold%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 119.4 ms | 131 ms | -8.81% |
| ❌ | Simulation | [`` ty_check_file[incremental] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_incremental%3A%3Aty_check_file%5Bincremental%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 4.8 ms | 6 ms | -20.06% |
| ❌ | Simulation | [`` ty_micro[complex_constrained_attributes_1] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_complex_constrained_attributes_1%3A%3Aty_micro%5Bcomplex_constrained_attributes_1%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 63 ms | 67.1 ms | -6.2% |
| ... | ... | ... | ... | ... | ... |

<br/>

> :information_source: _Only the first 20 benchmarks are displayed. [Go to the app to view all benchmarks](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fcodspeed-profile?utm_source=github&utm_medium=comment&utm_content=all_benchmarks)._


---

_Comment by @github-actions[bot] on 2025-11-03 18:55_

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

_@MichaReiser approved on 2025-11-03 20:40_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:952 on 2025-11-03 20:40_

You probably want to change the profile for other jobs too? Because this is only for the formatter.

---

_@MichaReiser reviewed on 2025-11-03 20:40_

---

_Comment by @ibraheemdev on 2025-11-04 14:59_

This reduces the walltime benchmarks from 15m to 10m, and we should see an even bigger improvement once build caching kicks in, so I think it's worth the downsides.

---

_Merged by @ibraheemdev on 2025-11-04 14:59_

---

_Closed by @ibraheemdev on 2025-11-04 14:59_

---

_Branch deleted on 2025-11-04 14:59_

---
