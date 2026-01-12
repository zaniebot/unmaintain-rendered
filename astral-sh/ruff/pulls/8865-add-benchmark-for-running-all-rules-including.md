```yaml
number: 8865
title: Add benchmark for running all rules including preview rules
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: benchmark-preview-rules
created_at: 2023-11-28T05:07:19Z
updated_at: 2023-11-29T04:26:26Z
url: https://github.com/astral-sh/ruff/pull/8865
synced_at: 2026-01-12T15:55:27Z
```

# Add benchmark for running all rules including preview rules

---

_@MichaReiser_

## Summary

This PR splits the linter all-rules benchmark into two:
* One that runs all stable rules
* One that runs all stable and preview rules

The motivation is that we may want to merge a preview rule before optimising its performance. With the new groups, unresolved performance regression show up at latest when promoting the rules to stable.

## Test Plan

I ran the benchmarks locally. Let's see if they get picked up by codspeed.


---

_Comment by @MichaReiser on 2023-11-28 05:07_

Current dependencies on/for this PR:
* main
  * **PR #8865** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8865?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8865?utm_source=stack-comment).

---

_Label `internal` added by @MichaReiser on 2023-11-28 05:08_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/linter.rs`:82 on 2023-11-28 05:19_

It seems that the benchmark was already running all preview rules, but the `preview_mode` was set to `Disabled`. Meaning, it doesn't set other preview-only changes to the linter.

---

_@MichaReiser reviewed on 2023-11-28 05:19_

---

_Comment by @codspeed-hq[bot] on 2023-11-28 05:27_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/benchmark-preview-rules)

### Merging #8865 will **improve performances by 10.88%**

<sub>Comparing <code>benchmark-preview-rules</code> (3ac4aab) with <code>main</code> (60eb11f)</sub>



### Summary

`⚡ 5` improvements
`✅ 20` untouched benchmarks

`🆕 5` new benchmarks



### Benchmarks breakdown

|     | Benchmark | `main` | `benchmark-preview-rules` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 4.1 ms | 3.9 ms | +6.5% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 79.3 ms | 71.6 ms | +10.88% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 36.7 ms | 34.3 ms | +6.91% |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 17.1 ms | 16.2 ms | +6.04% |
| ⚡ | `linter/all-rules[large/dataset.py]` | 183.8 ms | 167 ms | +10.04% |
| 🆕 | `linter/all-with-preview-rules[numpy/globals.py]` | N/A | 4.2 ms | N/A |
| 🆕 | `linter/all-with-preview-rules[unicode/pypinyin.py]` | N/A | 17.2 ms | N/A |
| 🆕 | `linter/all-with-preview-rules[pydantic/types.py]` | N/A | 78.7 ms | N/A |
| 🆕 | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | N/A | 37 ms | N/A |
| 🆕 | `linter/all-with-preview-rules[large/dataset.py]` | N/A | 184.1 ms | N/A |


---

_Comment by @github-actions[bot] on 2023-11-28 05:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@konstin approved on 2023-11-28 07:49_

---

_Merged by @MichaReiser on 2023-11-29 04:26_

---

_Closed by @MichaReiser on 2023-11-29 04:26_

---

_Branch deleted on 2023-11-29 04:26_

---
