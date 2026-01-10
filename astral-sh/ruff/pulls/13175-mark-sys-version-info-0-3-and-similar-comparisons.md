```yaml
number: 13175
title: "Mark `sys.version_info[0] < 3` and similar comparisons as outdated"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/sys
created_at: 2024-08-30T23:19:35Z
updated_at: 2024-08-30T23:38:48Z
url: https://github.com/astral-sh/ruff/pull/13175
synced_at: 2026-01-10T21:38:32Z
```

# Mark `sys.version_info[0] < 3` and similar comparisons as outdated

---

_Pull request opened by @charliermarsh on 2024-08-30 23:19_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12993.


---

_Label `rule` added by @charliermarsh on 2024-08-30 23:19_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:166 on 2024-08-30 23:20_

`== 3` is maybe a little dubious, but we were already doing it. (`==` is the _only_ case we handled before.)

---

_@charliermarsh reviewed on 2024-08-30 23:20_

---

_Comment by @codspeed-hq[bot] on 2024-08-30 23:25_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/sys)

### Merging #13175 will **degrade performances by 7.61%**

<sub>Comparing <code>charlie/sys</code> (b813d91) with <code>main</code> (28ab5f4)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/sys)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/sys` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 726.4 µs | 786.3 µs | -7.61% |


---

_Comment by @github-actions[bot] on 2024-08-30 23:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-08-30 23:38_

---

_Closed by @charliermarsh on 2024-08-30 23:38_

---

_Branch deleted on 2024-08-30 23:38_

---
