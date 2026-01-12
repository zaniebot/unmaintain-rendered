```yaml
number: 5772
title: "Move function visit out of `Expr::Call` branches"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/visit
created_at: 2023-07-15T03:30:25Z
updated_at: 2023-07-15T03:57:05Z
url: https://github.com/astral-sh/ruff/pull/5772
synced_at: 2026-01-12T03:30:21Z
```

# Move function visit out of `Expr::Call` branches

---

_Pull request opened by @charliermarsh on 2023-07-15 03:30_

## Summary

Non-behavioral change, but this is the same in each branch. Visiting the `func` first also means we've visited the `func` by the time we try to resolve it (via `resolve_call_path`), which should be helpful in a future refactor.


---

_@zanieb approved on 2023-07-15 03:32_

---

_Merged by @charliermarsh on 2023-07-15 03:36_

---

_Closed by @charliermarsh on 2023-07-15 03:36_

---

_Branch deleted on 2023-07-15 03:36_

---

_Comment by @github-actions[bot] on 2023-07-15 03:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.04ms     4.3 MB/sec    1.01      9.5±0.04ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1856.6±5.53µs     9.0 MB/sec    1.00   1865.2±2.21µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    209.9±0.38µs    14.1 MB/sec    1.00    210.5±1.07µs    14.0 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     13.8±0.08ms     3.0 MB/sec    1.00     13.7±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.8 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    452.8±0.55µs     6.5 MB/sec    1.00    452.7±3.44µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.03ms     4.2 MB/sec    1.00      6.1±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1483.7±3.81µs    11.2 MB/sec    1.00   1485.8±1.44µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.7±0.72µs    17.3 MB/sec    1.00    171.5±3.45µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.19ms     3.6 MB/sec    1.00     11.2±0.13ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.02      2.2±0.06ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    245.0±4.11µs    12.0 MB/sec    1.03   251.4±10.91µs    11.7 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.07ms     5.4 MB/sec    1.02      4.8±0.07ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     16.3±0.17ms     2.5 MB/sec    1.00     16.3±0.18ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.13ms     3.9 MB/sec    1.00      4.2±0.08ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    508.4±6.44µs     5.8 MB/sec    1.01   515.6±10.03µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.5±0.92ms     3.4 MB/sec    1.00      7.4±0.11ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.59ms     4.9 MB/sec    1.00      8.2±0.09ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1730.1±17.70µs     9.6 MB/sec    1.01  1741.8±25.12µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.2±2.95µs    14.6 MB/sec    1.00    202.2±6.83µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.00      3.7±0.05ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
