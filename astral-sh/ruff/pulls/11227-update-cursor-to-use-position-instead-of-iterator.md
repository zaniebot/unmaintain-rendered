```yaml
number: 11227
title: "Update `Cursor` to use position instead of iterator"
type: pull_request
state: closed
author: dhruvmanila
labels: []
assignees: []
draft: true
base: main
head: dhruv/cursor-position
created_at: 2024-05-01T03:55:48Z
updated_at: 2024-05-02T02:02:17Z
url: https://github.com/astral-sh/ruff/pull/11227
synced_at: 2026-01-12T15:55:37Z
```

# Update `Cursor` to use position instead of iterator

---

_@dhruvmanila_

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

_Comment by @codspeed-hq[bot] on 2024-05-01 04:00_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/cursor-position)

### Merging #11227 will **degrade performances by 24.36%**

<sub>Comparing <code>dhruv/cursor-position</code> (ea48d10) with <code>main</code> (414990c)</sub>



### Summary

`❌ 10` regressions
`✅ 20` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/cursor-position)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/cursor-position` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `lexer[large/dataset.py]` | 8.5 ms | 10.8 ms | -21.41% |
| ❌ | `lexer[numpy/ctypeslib.py]` | 1.6 ms | 2.1 ms | -23.99% |
| ❌ | `lexer[numpy/globals.py]` | 153.9 µs | 198.4 µs | -22.42% |
| ❌ | `lexer[pydantic/types.py]` | 3.7 ms | 4.9 ms | -24.36% |
| ❌ | `lexer[unicode/pypinyin.py]` | 527 µs | 667.4 µs | -21.03% |
| ❌ | `parser[large/dataset.py]` | 29.4 ms | 31.7 ms | -7.41% |
| ❌ | `parser[numpy/ctypeslib.py]` | 5.3 ms | 5.8 ms | -8.67% |
| ❌ | `parser[numpy/globals.py]` | 444.2 µs | 488.5 µs | -9.07% |
| ❌ | `parser[pydantic/types.py]` | 11.4 ms | 12.6 ms | -9.49% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.7 ms | 1.8 ms | -7.66% |


---

_Comment by @dhruvmanila on 2024-05-01 04:00_

Yup, thought so, let me try something else.

---

_Comment by @github-actions[bot] on 2024-05-01 04:13_

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

_@MichaReiser reviewed on 2024-05-01 06:32_

One possibility would be to keep `Cursor` as is and instead construct a new `Cursor` when restoring a `Lexer` snapshot (where the lexer snapshot stores the offset)

---

_Comment by @dhruvmanila on 2024-05-02 02:02_

Closing this as it's not relevant right now.

---

_Closed by @dhruvmanila on 2024-05-02 02:02_

---
