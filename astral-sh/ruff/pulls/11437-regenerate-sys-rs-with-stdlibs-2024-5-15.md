```yaml
number: 11437
title: Regenerate sys.rs with stdlibs==2024.5.15
type: pull_request
state: merged
author: thatch
labels:
  - isort
assignees: []
merged: true
base: main
head: new-stdlibs-3.13.0b1
created_at: 2024-05-15T21:50:52Z
updated_at: 2024-05-15T22:17:43Z
url: https://github.com/astral-sh/ruff/pull/11437
synced_at: 2026-01-10T22:05:26Z
```

# Regenerate sys.rs with stdlibs==2024.5.15

---

_Pull request opened by @thatch on 2024-05-15 21:50_

## Summary

Now that 3.13.0 b1 is out some of the stdlib modules have changed names.

## Test Plan

Wait for CI to run, expected to be pretty safe.

---

_Comment by @codspeed-hq[bot] on 2024-05-15 21:55_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/thatch:new-stdlibs-3.13.0b1)

### Merging #11437 will **degrade performances by 6.67%**

<sub>Comparing <code>thatch:new-stdlibs-3.13.0b1</code> (46198d8) with <code>main</code> (7ac9cab)</sub>



### Summary

`❌ 2` regressions
`✅ 28` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/thatch:new-stdlibs-3.13.0b1)._

### Benchmarks breakdown

|     | Benchmark | `main` | `thatch:new-stdlibs-3.13.0b1` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 5.2 ms | 5.6 ms | -6.67% |
| ❌ | `parser[pydantic/types.py]` | 11.2 ms | 11.8 ms | -4.49% |


---

_@charliermarsh approved on 2024-05-15 22:01_

---

_Label `isort` added by @charliermarsh on 2024-05-15 22:01_

---

_Comment by @github-actions[bot] on 2024-05-15 22:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2024-05-15 22:10_

I’ve never seen that Windows failure before. Gonna rerun…

---

_Merged by @charliermarsh on 2024-05-15 22:17_

---

_Closed by @charliermarsh on 2024-05-15 22:17_

---
