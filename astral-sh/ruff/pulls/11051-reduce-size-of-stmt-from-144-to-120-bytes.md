```yaml
number: 11051
title: Reduce size of Stmt from 144 to 120 bytes
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: main
head: box-type-params
created_at: 2024-04-19T22:45:05Z
updated_at: 2024-04-19T23:02:18Z
url: https://github.com/astral-sh/ruff/pull/11051
synced_at: 2026-01-12T15:55:34Z
```

# Reduce size of Stmt from 144 to 120 bytes

---

_@carljm_

## Summary

I happened to notice that we box `TypeParams` on `StmtClassDef` but not on `StmtFunctionDef` and wondered why, since `StmtFunctionDef` is bigger and sets the size of `Stmt`.

@charliermarsh found that at the time we started boxing type params on classes, classes were the largest statement type (see #6275), but that's no longer true.

So boxing type-params also on functions reduces the overall size of `Stmt`.

## Test Plan

The `<=` size tests are a bit irritating (since their failure doesn't tell you the actual size), but I manually confirmed that the size is actually 120 now.

---

_Label `internal` added by @carljm on 2024-04-19 22:45_

---

_Review requested from @charliermarsh by @carljm on 2024-04-19 22:45_

---

_Review requested from @MichaReiser by @carljm on 2024-04-19 22:45_

---

_Review requested from @dhruvmanila by @carljm on 2024-04-19 22:45_

---

_Comment by @codspeed-hq[bot] on 2024-04-19 22:50_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/box-type-params)

### Merging #11051 will **improve performances by 6.13%**

<sub>Comparing <code>box-type-params</code> (74b2d51) with <code>main</code> (99f7f94)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `box-type-params` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[unicode/pypinyin.py]` | 1.7 ms | 1.6 ms | +6.13% |


---

_@charliermarsh approved on 2024-04-19 22:50_

Thanks!

---

_Comment by @github-actions[bot] on 2024-04-19 23:02_

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

_Merged by @carljm on 2024-04-19 23:02_

---

_Closed by @carljm on 2024-04-19 23:02_

---

_Branch deleted on 2024-04-19 23:02_

---
