```yaml
number: 5985
title: "Do not raise `SIM105` for non-exceptions"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
assignees: []
merged: true
base: main
head: sim105-no-exception
created_at: 2023-07-22T15:56:36Z
updated_at: 2023-07-22T20:09:48Z
url: https://github.com/astral-sh/ruff/pull/5985
synced_at: 2026-01-12T03:30:22Z
```

# Do not raise `SIM105` for non-exceptions

---

_Pull request opened by @sbrugman on 2023-07-22 15:56_

Closes https://github.com/astral-sh/ruff/issues/5977

Added a test case from `refurb`

---

_Comment by @github-actions[bot] on 2023-07-22 16:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.9±0.03ms     4.1 MB/sec    1.00      9.8±0.02ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   1925.3±3.51µs     8.6 MB/sec    1.00   1899.6±3.57µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    206.9±0.37µs    14.3 MB/sec    1.00    206.5±0.79µs    14.3 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.01ms     6.0 MB/sec    1.00      4.2±0.00ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.05ms     3.0 MB/sec    1.01     13.7±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.8 MB/sec    1.00      3.4±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    373.5±1.05µs     7.9 MB/sec    1.00    375.3±1.12µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.8 MB/sec    1.00      7.1±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1433.1±5.96µs    11.6 MB/sec    1.00   1433.9±3.55µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.8±0.42µs    19.7 MB/sec    1.01    151.0±4.91µs    19.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.1 MB/sec    1.01      3.2±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.08ms     3.8 MB/sec    1.02     11.0±0.19ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.03ms     7.8 MB/sec    1.01      2.1±0.04ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    239.0±4.47µs    12.3 MB/sec    1.03   245.7±12.75µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.05ms     5.5 MB/sec    1.05      4.9±0.41ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.01     15.2±0.12ms     2.7 MB/sec    1.00     15.1±0.11ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.00      4.0±0.04ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   490.1±27.54µs     6.0 MB/sec    1.00    489.6±6.13µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.00      6.9±0.10ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.08ms     5.1 MB/sec    1.00      7.9±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1654.2±14.89µs    10.1 MB/sec    1.00  1655.9±28.46µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.4±3.04µs    15.4 MB/sec    1.02   195.7±15.55µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.03ms     7.2 MB/sec    1.01      3.6±0.06ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-07-22 18:30_

---

_Merged by @charliermarsh on 2023-07-22 18:36_

---

_Closed by @charliermarsh on 2023-07-22 18:36_

---

_Branch deleted on 2023-07-22 20:09_

---
