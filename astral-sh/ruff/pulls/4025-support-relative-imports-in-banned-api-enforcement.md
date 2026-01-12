```yaml
number: 4025
title: "Support relative imports in `banned-api` enforcement"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/relative-banned-api-ii
created_at: 2023-04-19T17:58:42Z
updated_at: 2023-04-19T18:43:05Z
url: https://github.com/astral-sh/ruff/pull/4025
synced_at: 2026-01-12T04:28:19Z
```

# Support relative imports in `banned-api` enforcement

---

_Pull request opened by @charliermarsh on 2023-04-19 17:58_

Closes #4013.


---

_Comment by @github-actions[bot] on 2023-04-19 18:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.2±0.02ms     2.7 MB/sec    1.00     15.1±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.3 MB/sec    1.00      3.8±0.00ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    416.0±2.85µs     7.1 MB/sec    1.00    415.8±1.81µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.01ms     4.0 MB/sec    1.00      6.5±0.01ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.01ms     5.2 MB/sec    1.01      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1728.3±2.50µs     9.6 MB/sec    1.01   1741.4±1.99µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    181.8±1.96µs    16.2 MB/sec    1.00    180.2±0.46µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.01      3.6±0.01ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.2±0.01ms     6.6 MB/sec    1.01      6.3±0.00ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1225.0±2.54µs    13.6 MB/sec    1.00   1226.2±0.94µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    124.0±1.20µs    23.8 MB/sec    1.00    123.4±0.35µs    23.9 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.01ms     9.5 MB/sec    1.01      2.7±0.00ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.5±0.23ms     2.5 MB/sec    1.00     16.4±0.28ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.15ms     3.9 MB/sec    1.00      4.2±0.16ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    499.6±8.58µs     5.9 MB/sec    1.00    496.4±8.92µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.16ms     3.6 MB/sec    1.01      7.1±0.24ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.13ms     4.9 MB/sec    1.01      8.3±0.15ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1823.8±25.27µs     9.1 MB/sec    1.03  1875.7±51.96µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.2±6.58µs    15.0 MB/sec    1.08   211.5±22.16µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.06      4.0±0.33ms     6.4 MB/sec
parser/large/dataset.py                    1.01      6.7±0.14ms     6.1 MB/sec    1.00      6.6±0.11ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.02  1266.8±27.37µs    13.1 MB/sec    1.00  1248.0±24.42µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    124.9±2.62µs    23.6 MB/sec    1.01   125.7±21.53µs    23.5 MB/sec
parser/pydantic/types.py                   1.02      2.8±0.03ms     9.0 MB/sec    1.00      2.8±0.04ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-19 18:30_

---

_Closed by @charliermarsh on 2023-04-19 18:30_

---

_Branch deleted on 2023-04-19 18:30_

---
