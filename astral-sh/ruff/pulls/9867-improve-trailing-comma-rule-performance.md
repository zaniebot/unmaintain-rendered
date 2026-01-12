```yaml
number: 9867
title: Improve trailing comma rule performance
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
assignees: []
merged: true
base: main
head: perf-trailing-comma
created_at: 2024-02-06T22:30:08Z
updated_at: 2024-02-06T23:05:58Z
url: https://github.com/astral-sh/ruff/pull/9867
synced_at: 2026-01-12T15:55:30Z
```

# Improve trailing comma rule performance

---

_@MichaReiser_

## Summary

I'm not sure what precisely improves performance but these are the things I did:

* I removed a lot of the iterator chaining. Before `flatten`, `filter_map`, `chain`, `coalesce`, `tuple_windows`. Now it's just `filter_map` I would have expected that Rust sees through that but maybe it does not. 
* I reordered the `TokenType` variants to match `Tok`.... I doubt that this has any impact
* I changed `Token::from` from `&Spanned` to `(&Tok, TextRange)`. 
* I avoid reading `context.last()` when we already set the context in the branch above

## Test Plan

`cargo test`


---

_Comment by @codspeed-hq[bot] on 2024-02-06 22:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/perf-trailing-comma)

### Merging #9867 will **improve performances by 10%**

<sub>Comparing <code>perf-trailing-comma</code> (f7076c4) with <code>main</code> (daae28e)</sub>



### Summary

`⚡ 10` improvements
`✅ 20` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `perf-trailing-comma` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[large/dataset.py]` | 92.5 ms | 84 ms | +10% |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 10.8 ms | 10.3 ms | +5.23% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 43.9 ms | 40 ms | +9.67% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 20.6 ms | 19.1 ms | +8.13% |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 2.8 ms | 2.7 ms | +5.63% |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 11.8 ms | 11.3 ms | +4.7% |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 3.1 ms | 2.9 ms | +5.02% |
| ⚡ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 23.3 ms | 21.6 ms | +7.41% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 105.1 ms | 96 ms | +9.48% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 51 ms | 47.4 ms | +7.77% |


---

_@charliermarsh approved on 2024-02-06 22:43_

I prefer your changes anyway lol.

---

_Label `performance` added by @MichaReiser on 2024-02-06 22:51_

---

_Renamed from "perf trailing comma" to "Improve trailing comma rule performance" by @MichaReiser on 2024-02-06 22:51_

---

_Marked ready for review by @MichaReiser on 2024-02-06 23:02_

---

_Merged by @MichaReiser on 2024-02-06 23:04_

---

_Closed by @MichaReiser on 2024-02-06 23:04_

---

_Branch deleted on 2024-02-06 23:04_

---

_Comment by @github-actions[bot] on 2024-02-06 23:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
