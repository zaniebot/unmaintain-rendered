```yaml
number: 9882
title: "Reduce `LexResult` size by boxing `LexicalError`"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: lexer-perf
created_at: 2024-02-07T21:51:07Z
updated_at: 2024-02-29T13:18:54Z
url: https://github.com/astral-sh/ruff/pull/9882
synced_at: 2026-01-10T22:47:01Z
```

# Reduce `LexResult` size by boxing `LexicalError`

---

_Pull request opened by @MichaReiser on 2024-02-07 21:51_

- Make `LexicalError` fields private
- Box `LexicalError` to reduce Result size


---

_Closed by @MichaReiser on 2024-02-07 22:01_

---

_Reopened by @MichaReiser on 2024-02-07 22:01_

---

_Comment by @codspeed-hq[bot] on 2024-02-07 22:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/lexer-perf)

### Merging #9882 will **degrade performances by 5.43%**

<sub>Comparing <code>lexer-perf</code> (1ec90fb) with <code>main</code> (4593742)</sub>



### Summary

`❌ 4` regressions
`✅ 26` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/lexer-perf)._

### Benchmarks breakdown

|     | Benchmark | `main` | `lexer-perf` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `lexer[unicode/pypinyin.py]` | 571.5 µs | 600.2 µs | -4.79% |
| ❌ | `lexer[numpy/ctypeslib.py]` | 1.8 ms | 1.9 ms | -4.51% |
| ❌ | `lexer[pydantic/types.py]` | 3.9 ms | 4.1 ms | -5.19% |
| ❌ | `lexer[large/dataset.py]` | 8.8 ms | 9.3 ms | -5.43% |


---

_Comment by @github-actions[bot] on 2024-02-07 23:17_

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

_Comment by @MichaReiser on 2024-02-08 02:33_

This regresses performance because it's now necessary to drop each `LexResult` 

---

_Closed by @MichaReiser on 2024-02-08 11:55_

---

_Branch deleted on 2024-02-29 13:18_

---
