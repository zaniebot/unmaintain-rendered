```yaml
number: 8986
title: Playing with some parser changes, ignore
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: parser-playfield
created_at: 2023-12-04T03:37:13Z
updated_at: 2024-01-16T10:02:55Z
url: https://github.com/astral-sh/ruff/pull/8986
synced_at: 2026-01-10T22:57:09Z
```

# Playing with some parser changes, ignore

---

_Pull request opened by @MichaReiser on 2023-12-04 03:37_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @MichaReiser on 2023-12-04 03:37_

Current dependencies on/for this PR:
* main
  * **PR #8986** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8986?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8986?utm_source=stack-comment).

---

_Comment by @codspeed-hq[bot] on 2023-12-04 03:47_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/parser-playfield)

### Merging #8986 will **degrade performances by 6.06%**

<sub>Comparing <code>parser-playfield</code> (c3da60a) with <code>main</code> (6fe8f8a)</sub>



### Summary

`❌ 6` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/parser-playfield)._

### Benchmarks breakdown

|     | Benchmark | `main` | `parser-playfield` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[unicode/pypinyin.py]` | 3.9 ms | 4.1 ms | -5.53% |
| ❌ | `parser[large/dataset.py]` | 64.9 ms | 68.8 ms | -5.71% |
| ❌ | `parser[numpy/globals.py]` | 1.1 ms | 1.1 ms | -4.98% |
| ❌ | `parser[numpy/ctypeslib.py]` | 11.1 ms | 11.8 ms | -5.6% |
| ❌ | `parser[pydantic/types.py]` | 24.8 ms | 26.3 ms | -5.64% |
| ❌ | `linter/default-rules[pydantic/types.py]` | 34.6 ms | 36.8 ms | -6.06% |


---

_Comment by @github-actions[bot] on 2023-12-04 03:53_

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

_Closed by @MichaReiser on 2023-12-04 05:11_

---

_Branch deleted on 2024-01-16 10:02_

---
