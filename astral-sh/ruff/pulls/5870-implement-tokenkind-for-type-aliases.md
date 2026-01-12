```yaml
number: 5870
title: "Implement `TokenKind` for type aliases"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/695-token-kind
created_at: 2023-07-18T18:14:12Z
updated_at: 2023-07-18T18:46:16Z
url: https://github.com/astral-sh/ruff/pull/5870
synced_at: 2026-01-12T03:30:22Z
```

# Implement `TokenKind` for type aliases

---

_Pull request opened by @zanieb on 2023-07-18 18:14_

Part of https://github.com/astral-sh/ruff/issues/5062

---

_@charliermarsh approved on 2023-07-18 18:14_

---

_Merged by @zanieb on 2023-07-18 18:21_

---

_Closed by @zanieb on 2023-07-18 18:21_

---

_Branch deleted on 2023-07-18 18:21_

---

_Comment by @github-actions[bot] on 2023-07-18 18:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.8±0.02ms     4.1 MB/sec    1.00      9.8±0.06ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.01  1912.3±17.05µs     8.7 MB/sec    1.00   1891.8±5.48µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    207.8±1.60µs    14.2 MB/sec    1.00    207.8±1.07µs    14.2 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.05ms     6.0 MB/sec    1.00      4.2±0.02ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.05ms     3.0 MB/sec    1.00     13.5±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.8 MB/sec    1.01      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    369.0±1.88µs     8.0 MB/sec    1.02    377.9±6.51µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.02ms     5.8 MB/sec    1.00      7.0±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1432.2±10.97µs    11.6 MB/sec    1.00   1435.1±4.88µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    148.7±0.29µs    19.8 MB/sec    1.00    149.0±0.26µs    19.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.1 MB/sec    1.00      3.1±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.8±0.12ms     3.4 MB/sec    1.00     11.8±0.18ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.02ms     7.4 MB/sec    1.01      2.3±0.03ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00    239.1±4.21µs    12.3 MB/sec    1.02   244.1±14.04µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.04ms     5.1 MB/sec    1.01      5.0±0.13ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.10ms     2.6 MB/sec    1.00     15.4±0.13ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.5±7.58µs     7.0 MB/sec    1.00    420.3±5.98µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.05ms     3.6 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.06ms     5.0 MB/sec    1.00      8.2±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1646.6±12.33µs    10.1 MB/sec    1.00  1650.8±15.05µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    174.6±1.87µs    16.9 MB/sec    1.00    173.5±1.50µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.03ms     7.0 MB/sec    1.00      3.6±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
