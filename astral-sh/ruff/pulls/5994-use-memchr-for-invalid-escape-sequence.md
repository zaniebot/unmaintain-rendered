```yaml
number: 5994
title: "Use `memchr` for `invalid-escape-sequence`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/comment
created_at: 2023-07-23T00:37:46Z
updated_at: 2023-07-23T01:13:38Z
url: https://github.com/astral-sh/ruff/pull/5994
synced_at: 2026-01-12T03:30:22Z
```

# Use `memchr` for `invalid-escape-sequence`

---

_Pull request opened by @charliermarsh on 2023-07-23 00:37_

_No description provided._

---

_Label `performance` added by @charliermarsh on 2023-07-23 00:38_

---

_Comment by @github-actions[bot] on 2023-07-23 00:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.02ms     4.4 MB/sec    1.01      9.4±0.01ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1844.8±2.11µs     9.0 MB/sec    1.01   1855.7±1.80µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    202.4±1.27µs    14.6 MB/sec    1.01    204.5±0.28µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.01      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     13.0±0.12ms     3.1 MB/sec    1.00     12.8±0.04ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.00ms     5.1 MB/sec    1.00      3.3±0.00ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.02    434.2±0.42µs     6.8 MB/sec    1.00    426.3±0.72µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      6.6±0.01ms     6.1 MB/sec    1.00      6.5±0.01ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1412.2±1.58µs    11.8 MB/sec    1.00   1394.0±1.25µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    156.5±0.58µs    18.9 MB/sec    1.00    154.2±0.25µs    19.1 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.0±0.01ms     8.5 MB/sec    1.00      2.9±0.01ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.17ms     4.1 MB/sec    1.00      9.8±0.16ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1932.0±34.10µs     8.6 MB/sec    1.00  1925.5±28.94µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    219.3±8.89µs    13.5 MB/sec    1.00   219.0±15.89µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.10ms     6.1 MB/sec    1.01      4.3±0.09ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.19ms     2.9 MB/sec    1.00     13.9±0.26ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.05ms     4.6 MB/sec    1.00      3.6±0.06ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.9±9.63µs     6.8 MB/sec    1.01   436.2±10.81µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.08ms     4.1 MB/sec    1.00      6.3±0.16ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.10ms     5.6 MB/sec    1.01      7.3±0.12ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1497.6±27.64µs    11.1 MB/sec    1.01  1518.1±31.78µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.1±3.90µs    17.4 MB/sec    1.00    169.9±4.76µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.05ms     8.0 MB/sec    1.01      3.2±0.06ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-23 00:57_

---

_Closed by @charliermarsh on 2023-07-23 00:57_

---

_Branch deleted on 2023-07-23 00:57_

---
