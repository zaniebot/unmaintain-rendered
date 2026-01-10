```yaml
number: 12863
title: "Avoid treating `dataclasses.KW_ONLY` as typing-only"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/kw-data
created_at: 2024-08-13T15:24:23Z
updated_at: 2024-08-13T18:34:58Z
url: https://github.com/astral-sh/ruff/pull/12863
synced_at: 2026-01-10T21:38:32Z
```

# Avoid treating `dataclasses.KW_ONLY` as typing-only

---

_Pull request opened by @charliermarsh on 2024-08-13 15:24_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12859.


---

_Label `bug` added by @charliermarsh on 2024-08-13 15:24_

---

_Comment by @codspeed-hq[bot] on 2024-08-13 15:30_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/kw-data)

### Merging #12863 will **degrade performances by 4.1%**

<sub>Comparing <code>charlie/kw-data</code> (d051027) with <code>main</code> (899a523)</sub>



### Summary

`⚡ 1` improvements
`❌ 1` regressions
`✅ 30` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/kw-data)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/kw-data` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 765 µs | 723.1 µs | +5.8% |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -4.1% |


---

_Comment by @github-actions[bot] on 2024-08-13 15:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-13 15:42_

---

_@AlexWaygood approved on 2024-08-13 15:48_

These special cases hurt so badly. But it's not our fault, just something we have to live with since Python's semantics are what they are!

PEP 649 can't land soon enough :(

---

_Merged by @charliermarsh on 2024-08-13 18:34_

---

_Closed by @charliermarsh on 2024-08-13 18:34_

---

_Branch deleted on 2024-08-13 18:34_

---
