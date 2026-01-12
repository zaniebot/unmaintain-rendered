```yaml
number: 4908
title: Set default Rust version when testing sdist
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rustup-sdist
created_at: 2023-06-06T20:47:24Z
updated_at: 2023-06-06T21:24:23Z
url: https://github.com/astral-sh/ruff/pull/4908
synced_at: 2026-01-12T03:43:29Z
```

# Set default Rust version when testing sdist

---

_Pull request opened by @charliermarsh on 2023-06-06 20:47_

## Test Plan

See: https://github.com/charliermarsh/ruff/actions/runs/5193203293/jobs/9363428303.

---

_Merged by @charliermarsh on 2023-06-06 20:59_

---

_Closed by @charliermarsh on 2023-06-06 20:59_

---

_Branch deleted on 2023-06-06 20:59_

---

_Comment by @charliermarsh on 2023-06-06 21:00_

\cc @konstin 

---

_Comment by @github-actions[bot] on 2023-06-06 21:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      5.7±0.02ms     7.1 MB/sec    1.00      5.7±0.01ms     7.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1193.4±2.26µs    14.0 MB/sec    1.00   1190.4±6.35µs    14.0 MB/sec
formatter/numpy/globals.py                 1.01    138.9±0.16µs    21.2 MB/sec    1.00    138.1±0.18µs    21.4 MB/sec
formatter/pydantic/types.py                1.00      2.5±0.01ms    10.1 MB/sec    1.00      2.5±0.01ms    10.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.15ms     2.9 MB/sec    1.00     14.2±0.15ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.0 MB/sec    1.01      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    414.2±0.65µs     7.1 MB/sec    1.00    415.6±0.79µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.05ms     4.4 MB/sec    1.01      5.9±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.06ms     6.0 MB/sec    1.02      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1456.1±2.85µs    11.4 MB/sec    1.01   1471.2±5.90µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.4±0.27µs    18.3 MB/sec    1.01    163.2±0.44µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.01      3.1±0.00ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.16     10.9±0.39ms     3.7 MB/sec    1.00      9.4±0.36ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.09      2.1±0.11ms     7.9 MB/sec    1.00  1930.5±70.84µs     8.6 MB/sec
formatter/numpy/globals.py                 1.05    228.7±9.29µs    12.9 MB/sec    1.00   218.2±27.85µs    13.5 MB/sec
formatter/pydantic/types.py                1.14      4.6±0.19ms     5.5 MB/sec    1.00      4.0±0.14ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     21.5±0.64ms  1936.3 KB/sec    1.07     23.1±0.77ms  1805.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.4±0.18ms     3.1 MB/sec    1.05      5.7±0.21ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   628.7±31.92µs     4.7 MB/sec    1.05   660.8±29.01µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.1±0.58ms     2.8 MB/sec    1.05      9.6±0.30ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.6±0.35ms     3.9 MB/sec    1.02     10.8±0.32ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.09ms     7.6 MB/sec    1.04      2.3±0.14ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    262.5±8.92µs    11.2 MB/sec    1.07   280.7±12.79µs    10.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.22ms     5.2 MB/sec    1.03      5.0±0.17ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
