```yaml
number: 11598
title: use owned ast and tokens in bench
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: use-owned-ast-and-tokens-in-bench
created_at: 2024-05-29T11:07:47Z
updated_at: 2024-05-29T16:10:33Z
url: https://github.com/astral-sh/ruff/pull/11598
synced_at: 2026-01-10T21:56:00Z
```

# use owned ast and tokens in bench

---

_Pull request opened by @MichaReiser on 2024-05-29 11:07_

## Summary

Use owned values for the linter benchmark as preparation for the parser refactor

The perf regressions come from the fact that the `tokens` and `ast` are now dropped inside the benchmark. I think this is good, because that is closer to what we see in production. 

---

_Label `internal` added by @MichaReiser on 2024-05-29 11:08_

---

_Comment by @codspeed-hq[bot] on 2024-05-29 11:17_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/use-owned-ast-and-tokens-in-bench)

### Merging #11598 will **degrade performances by 15.62%**

<sub>Comparing <code>use-owned-ast-and-tokens-in-bench</code> (9517a4d) with <code>main</code> (7659114)</sub>



### Summary

`❌ 5 (👁 5)` regressions
`✅ 25` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `use-owned-ast-and-tokens-in-bench` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `linter/all-rules[large/dataset.py]` | 16.2 ms | 16.9 ms | -4.32% |
| 👁 | `linter/default-rules[large/dataset.py]` | 3.6 ms | 4.3 ms | -15.62% |
| 👁 | `linter/default-rules[numpy/ctypeslib.py]` | 904.6 µs | 1,011.7 µs | -10.58% |
| 👁 | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 2.1 ms | -12.29% |
| 👁 | `linter/default-rules[unicode/pypinyin.py]` | 350.7 µs | 405.2 µs | -13.47% |


---

_Comment by @github-actions[bot] on 2024-05-29 11:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dhruvmanila by @MichaReiser on 2024-05-29 11:41_

---

_@dhruvmanila reviewed on 2024-05-29 15:19_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:1 on 2024-05-29 15:19_

What are the changes in this file? Is it related to what's changed in this PR?

---

_@dhruvmanila approved on 2024-05-29 15:19_

Thanks!

---

_Merged by @MichaReiser on 2024-05-29 16:10_

---

_Closed by @MichaReiser on 2024-05-29 16:10_

---

_Branch deleted on 2024-05-29 16:10_

---
