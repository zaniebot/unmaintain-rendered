```yaml
number: 6002
title: "Remove extra `error!` call"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/error
created_at: 2023-07-23T02:22:03Z
updated_at: 2023-07-23T02:57:35Z
url: https://github.com/astral-sh/ruff/pull/6002
synced_at: 2026-01-12T03:30:22Z
```

# Remove extra `error!` call

---

_Pull request opened by @charliermarsh on 2023-07-23 02:22_

_No description provided._

---

_Marked ready for review by @charliermarsh on 2023-07-23 02:22_

---

_Label `internal` added by @charliermarsh on 2023-07-23 02:26_

---

_Merged by @charliermarsh on 2023-07-23 02:29_

---

_Closed by @charliermarsh on 2023-07-23 02:29_

---

_Branch deleted on 2023-07-23 02:29_

---

_Comment by @github-actions[bot] on 2023-07-23 02:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.03ms     4.4 MB/sec    1.01      9.4±0.04ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1849.8±3.53µs     9.0 MB/sec    1.00   1855.1±4.29µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    203.0±0.30µs    14.5 MB/sec    1.00    203.4±2.45µs    14.5 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.00ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     13.2±0.09ms     3.1 MB/sec    1.00     13.1±0.07ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.9±0.95µs     6.8 MB/sec    1.00    431.0±0.59µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.2 MB/sec    1.00      6.6±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1414.4±1.60µs    11.8 MB/sec    1.00   1411.4±2.28µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.0±0.27µs    18.9 MB/sec    1.00    156.0±0.80µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.14ms     3.6 MB/sec    1.00     11.2±0.23ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.00      2.2±0.04ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    242.6±4.32µs    12.2 MB/sec    1.02   247.4±10.88µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.07ms     5.4 MB/sec    1.00      4.8±0.07ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.18ms     2.5 MB/sec    1.00     16.0±0.19ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.2±0.29ms     4.0 MB/sec    1.00      4.1±0.07ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.1±8.26µs     6.0 MB/sec    1.00    490.0±7.85µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.14ms     3.5 MB/sec    1.00      7.2±0.14ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.09ms     5.0 MB/sec    1.01      8.3±0.12ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1688.9±22.04µs     9.9 MB/sec    1.00  1693.3±22.24µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.4±4.06µs    15.4 MB/sec    1.00    191.2±3.40µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.05ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
