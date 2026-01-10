```yaml
number: 9920
title: "Reduce size of `Expr` enum"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/expr-size
created_at: 2024-02-10T02:59:22Z
updated_at: 2024-03-12T19:48:33Z
url: https://github.com/astral-sh/ruff/pull/9920
synced_at: 2026-01-10T22:47:01Z
```

# Reduce size of `Expr` enum

---

_Pull request opened by @charliermarsh on 2024-02-10 02:59_

Okay, this is the last one. Mostly just curious if it helps at all.

---

_Comment by @codspeed-hq[bot] on 2024-02-10 03:06_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/expr-size)

### Merging #9920 will **degrade performances by 5.02%**

<sub>Comparing <code>charlie/expr-size</code> (4e23ee0) with <code>main</code> (8ec5627)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/expr-size)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/expr-size` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `formatter[large/dataset.py]` | 53.2 ms | 56 ms | -5.02% |


---

_Comment by @charliermarsh on 2024-02-10 03:08_

Not sure on this one, it does have the potential to degrade performance since we're now boxing the `Arguments` on `ExprCall`.

---

_Comment by @charliermarsh on 2024-02-10 19:26_

The net effect here (after running a bunch of benchmarks locally) seems like be a ~2-3% parser speed-up, and a ~2-3% linter slowdown.

---

_Closed by @charliermarsh on 2024-03-12 19:48_

---
