```yaml
number: 11666
title: "Omit `red-knot` PRs from the changelog"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/red
created_at: 2024-05-31T23:13:59Z
updated_at: 2024-05-31T23:18:55Z
url: https://github.com/astral-sh/ruff/pull/11666
synced_at: 2026-01-12T15:55:38Z
```

# Omit `red-knot` PRs from the changelog

---

_@charliermarsh_

## Summary

This just ensures that PRs labelled with `red-knot` are automatically filtered out from the auto-generated changelog (which we then manually finalize anyway).


---

_Renamed from "Omit red-knot PRs from the changelog" to "Omit `red-knot` PRs from the changelog" by @charliermarsh on 2024-05-31 23:14_

---

_Label `internal` added by @charliermarsh on 2024-05-31 23:14_

---

_Review requested from @carljm by @charliermarsh on 2024-05-31 23:14_

---

_@carljm approved on 2024-05-31 23:18_

---

_Comment by @codspeed-hq[bot] on 2024-05-31 23:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/red)

### Merging #11666 will **degrade performances by 9.91%**

<sub>Comparing <code>charlie/red</code> (ccacf47) with <code>main</code> (312f664)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/red)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/red` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 1 ms | 1.2 ms | -9.91% |


---

_Merged by @charliermarsh on 2024-05-31 23:18_

---

_Closed by @charliermarsh on 2024-05-31 23:18_

---

_Branch deleted on 2024-05-31 23:18_

---
