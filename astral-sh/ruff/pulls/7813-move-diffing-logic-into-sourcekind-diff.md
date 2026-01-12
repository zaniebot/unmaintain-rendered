```yaml
number: 7813
title: "Move diffing logic into `SourceKind::diff`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/source-kind
created_at: 2023-10-04T14:23:21Z
updated_at: 2023-10-04T15:15:41Z
url: https://github.com/astral-sh/ruff/pull/7813
synced_at: 2026-01-12T15:55:24Z
```

# Move diffing logic into `SourceKind::diff`

---

_@charliermarsh_

_No description provided._

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-10-04 14:23_

---

_Label `internal` added by @charliermarsh on 2023-10-04 14:23_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/source_kind.rs`:91 on 2023-10-04 14:36_

nit: maybe we could get the writer from the caller which could potentially allow us to write tests in terms of snapshots.

---

_@dhruvmanila approved on 2023-10-04 14:37_

---

_Comment by @github-actions[bot] on 2023-10-04 14:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @charliermarsh on 2023-10-04 15:08_

---

_Closed by @charliermarsh on 2023-10-04 15:08_

---

_Branch deleted on 2023-10-04 15:08_

---

_Comment by @codspeed-hq[bot] on 2023-10-04 15:15_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/source-kind)

### Merging #7813 will **degrade performances by 3.38%**

<sub>Comparing <code>charlie/source-kind</code> (fdd3b2b) with <code>main</code> (e674e87)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/source-kind)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/source-kind` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 92.8 ms | 96 ms | -3.38% |


---
