```yaml
number: 7745
title: Decrease PEP 593 error to a debug warning
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/warning
created_at: 2023-10-01T18:27:20Z
updated_at: 2023-10-01T18:43:09Z
url: https://github.com/astral-sh/ruff/pull/7745
synced_at: 2026-01-12T02:39:10Z
```

# Decrease PEP 593 error to a debug warning

---

_Pull request opened by @charliermarsh on 2023-10-01 18:27_

## Summary

There's no way for users to fix this warning if they're intentionally using an "invalid" PEP 593 annotation, as is the case in CPython. This is a symptom of having warnings that aren't themselves diagnostics. If we want this to be user-facing, we should add a diagnostic for it!

## Test Plan

Ran `cargo run -p ruff_cli -- check foo.py -n` on:

```python
from typing import Annotated

Annotated[int]
```


---

_Label `cli` added by @charliermarsh on 2023-10-01 18:28_

---

_Comment by @codspeed-hq[bot] on 2023-10-01 18:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/warning)

### Merging #7745 will **improve performances by 2.02%**

<sub>Comparing <code>charlie/warning</code> (ceb74b6) with <code>main</code> (d8a6279)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/warning` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 19 ms | 18.6 ms | +2.02% |


---

_Merged by @charliermarsh on 2023-10-01 18:40_

---

_Closed by @charliermarsh on 2023-10-01 18:40_

---

_Branch deleted on 2023-10-01 18:40_

---

_Comment by @github-actions[bot] on 2023-10-01 18:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
