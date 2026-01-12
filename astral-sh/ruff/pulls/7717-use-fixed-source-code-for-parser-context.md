```yaml
number: 7717
title: Use fixed source code for parser context
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/parse
created_at: 2023-09-29T17:48:27Z
updated_at: 2023-09-29T18:10:34Z
url: https://github.com/astral-sh/ruff/pull/7717
synced_at: 2026-01-12T02:39:10Z
```

# Use fixed source code for parser context

---

_Pull request opened by @charliermarsh on 2023-09-29 17:48_

## Summary

The parser now uses the raw source code as global context and slices into it to parse debug text. It turns out we were always passing in the _old_ source code, so when code was fixed, we were making invalid accesses. This PR modifies the call to use the _fixed_ source code, which will always be consistent with the tokens.

Closes https://github.com/astral-sh/ruff/issues/7711.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-09-29 17:48_

---

_Comment by @codspeed-hq[bot] on 2023-09-29 17:58_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/parse)

### Merging #7717 will **degrade performances by 3.89%**

<sub>Comparing <code>charlie/parse</code> (185f309) with <code>main</code> (b42a897)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/parse)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/parse` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 92 ms | 95.7 ms | -3.89% |


---

_Comment by @github-actions[bot] on 2023-09-29 18:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb approved on 2023-09-29 18:06_

---

_Merged by @charliermarsh on 2023-09-29 18:10_

---

_Closed by @charliermarsh on 2023-09-29 18:10_

---

_Branch deleted on 2023-09-29 18:10_

---
