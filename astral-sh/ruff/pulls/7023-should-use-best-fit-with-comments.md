```yaml
number: 7023
title: "`should_use_best_fit with comments"
type: pull_request
state: closed
author: davidszotten
labels: []
assignees: []
draft: true
base: main
head: should-use-best-fit-with-comments
created_at: 2023-08-31T14:37:18Z
updated_at: 2023-09-20T06:42:51Z
url: https://github.com/astral-sh/ruff/pull/7023
synced_at: 2026-01-12T02:39:09Z
```

# `should_use_best_fit with comments

---

_Pull request opened by @davidszotten on 2023-08-31 14:37_

draft for discussion

also use trailing comment length in the `should_use_best_fit` text length heuristic

https://github.com/astral-sh/ruff/issues/5630#issuecomment-1700532155

currently unstable

---

_Comment by @codspeed-hq[bot] on 2023-08-31 14:48_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/davidszotten:should-use-best-fit-with-comments)

### Merging #7023 will **degrade performances by 2.9%**

<sub>Comparing <code>davidszotten:should-use-best-fit-with-comments</code> (0b55fbd) with <code>main</code> (f4ba0ea)</sub>



### Summary

`❌ 1` regressions
`✅ 19` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/davidszotten:should-use-best-fit-with-comments)._

### Benchmarks breakdown

|     | Benchmark | `main` | `davidszotten:should-use-best-fit-with-comments` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 91.5 ms | 94.3 ms | -2.9% |


---

_Comment by @cnpryer on 2023-08-31 16:09_

Is this related? https://github.com/astral-sh/ruff/pull/6830#issuecomment-1694268269

---

_Comment by @MichaReiser on 2023-09-20 06:42_

This is fixed with https://github.com/astral-sh/ruff/pull/7475

---

_Closed by @MichaReiser on 2023-09-20 06:42_

---
