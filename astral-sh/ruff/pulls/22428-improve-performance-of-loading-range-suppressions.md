```yaml
number: 22428
title: Improve performance of loading range suppressions
type: pull_request
state: closed
author: amyreese
labels:
  - internal
assignees: []
draft: true
base: main
head: amy/suppression-perf
created_at: 2026-01-07T02:34:34Z
updated_at: 2026-01-08T19:17:43Z
url: https://github.com/astral-sh/ruff/pull/22428
synced_at: 2026-01-12T15:57:49Z
```

# Improve performance of loading range suppressions

---

_@amyreese_

WIP, not fully baked, checkpointing progress:

- creates new block range indexer to track indent/dedent ranges
- pass indexer to suppression loader
- new implementation of loading from tokens based on comment ranges and block range index
- doesn't need to walk tokens forward/backward looking for matching indent scopes
- has issue with first comment in block preceding the indent token, and block range index not knowing

issue #22087

---

_Comment by @astral-sh-bot[bot] on 2026-01-07 02:43_


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

_Comment by @codspeed-hq[bot] on 2026-01-07 02:43_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/amy%2Fsuppression-perf?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 17.64%**

<sub>Comparing <code>amy/suppression-perf</code> (c7b528e) with <code>main</code> (f97da18)</sub>



### Summary

`❌ 5` regressed benchmarks  
`✅ 48` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/amy%2Fsuppression-perf?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | Simulation | [`` linter/all-with-preview-rules[numpy/globals.py] ``](https://codspeed.io/astral-sh/ruff/branches/amy%2Fsuppression-perf?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bnumpy%2Fglobals.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 831.7 µs | 914.3 µs | -9.04% |
| ❌ | Simulation | [`` linter/all-with-preview-rules[unicode/pypinyin.py] ``](https://codspeed.io/astral-sh/ruff/branches/amy%2Fsuppression-perf?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bunicode%2Fpypinyin.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.2 ms | 2.7 ms | -17.64% |
| ❌ | Simulation | [`` linter/all-with-preview-rules[numpy/ctypeslib.py] ``](https://codspeed.io/astral-sh/ruff/branches/amy%2Fsuppression-perf?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bnumpy%2Fctypeslib.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 5.2 ms | 5.7 ms | -8.8% |
| ❌ | Simulation | [`` linter/all-with-preview-rules[large/dataset.py] ``](https://codspeed.io/astral-sh/ruff/branches/amy%2Fsuppression-perf?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Blarge%2Fdataset.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 22.3 ms | 23.5 ms | -5.14% |
| ❌ | Simulation | [`` linter/all-with-preview-rules[pydantic/types.py] ``](https://codspeed.io/astral-sh/ruff/branches/amy%2Fsuppression-perf?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bpydantic%2Ftypes.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 10.2 ms | 11 ms | -7.48% |


---

_Renamed from "improve performance of loading range suppressions" to "Improve performance of loading range suppressions" by @amyreese on 2026-01-07 02:46_

---

_Label `internal` added by @amyreese on 2026-01-07 02:46_

---

_Comment by @amyreese on 2026-01-07 02:48_

codspeed not happy 🤪 

---

_Comment by @MichaReiser on 2026-01-07 07:33_

> codspeed not happy 🤪

`dbg` statements are performance killers

---

_@MichaReiser reviewed on 2026-01-07 07:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_index/src/indexer.rs`:31 on 2026-01-07 07:36_

I prefer avoiding adding new state to `Indexer` because it's state that we can't release until the file has finished checking. That's why we generally prefer re-computing information unless doing so is expensive or has to be repeated in many places (recomputing can often also be cheaper). 

Are there cases where the implementation in https://github.com/astral-sh/ruff/pull/21441#pullrequestreview-3503773083 falls short, making computing this information ahead of time necessary?

---

_Closed by @amyreese on 2026-01-08 19:17_

---
