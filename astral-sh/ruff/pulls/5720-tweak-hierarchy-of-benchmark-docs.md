```yaml
number: 5720
title: Tweak hierarchy of benchmark docs
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/benchmark-docs
created_at: 2023-07-12T20:34:17Z
updated_at: 2023-07-12T21:08:24Z
url: https://github.com/astral-sh/ruff/pull/5720
synced_at: 2026-01-12T03:36:55Z
```

# Tweak hierarchy of benchmark docs

---

_Pull request opened by @charliermarsh on 2023-07-12 20:34_

## Summary

Before:

<img width="309" alt="Screen Shot 2023-07-12 at 4 33 23 PM" src="https://github.com/astral-sh/ruff/assets/1309177/b4a29dc5-183d-479f-8028-f47157b87e0e">

After:

<img width="281" alt="Screen Shot 2023-07-12 at 4 33 32 PM" src="https://github.com/astral-sh/ruff/assets/1309177/316859d3-db90-4595-8c07-b4bb6543ac4d">


---

_Review requested from @konstin by @charliermarsh on 2023-07-12 20:34_

---

_Label `documentation` added by @charliermarsh on 2023-07-12 20:34_

---

_Comment by @github-actions[bot] on 2023-07-12 20:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.3±0.34ms     4.4 MB/sec    1.00      9.1±0.28ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.1±0.08ms     7.8 MB/sec    1.00      2.1±0.07ms     8.0 MB/sec
formatter/numpy/globals.py                 1.02    238.5±9.38µs    12.4 MB/sec    1.00   234.9±10.60µs    12.6 MB/sec
formatter/pydantic/types.py                1.04      4.7±0.17ms     5.5 MB/sec    1.00      4.5±0.17ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.45ms     2.6 MB/sec    1.01     15.7±0.39ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.11ms     4.3 MB/sec    1.00      3.9±0.12ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.03   505.2±22.19µs     5.8 MB/sec    1.00   492.1±20.57µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.23ms     3.6 MB/sec    1.00      6.9±0.19ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      7.6±0.18ms     5.3 MB/sec    1.00      7.6±0.14ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1641.3±44.66µs    10.1 MB/sec    1.00  1646.2±55.38µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    197.6±8.52µs    14.9 MB/sec    1.00    195.3±9.98µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.11ms     7.4 MB/sec    1.00      3.4±0.14ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.08ms     4.3 MB/sec    1.00      9.5±0.09ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.04ms     7.7 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    240.2±4.63µs    12.3 MB/sec    1.01    243.1±8.07µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.07ms     5.4 MB/sec    1.00      4.7±0.07ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.1±0.14ms     2.5 MB/sec    1.00     16.2±0.14ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.00      4.2±0.17ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    504.8±8.30µs     5.8 MB/sec    1.00    502.5±8.75µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.07ms     3.5 MB/sec    1.00      7.2±0.07ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.01      8.2±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1719.1±32.35µs     9.7 MB/sec    1.00  1721.7±15.41µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.2±3.28µs    14.9 MB/sec    1.01    199.2±2.83µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-12 20:54_

---

_Merged by @charliermarsh on 2023-07-12 21:08_

---

_Closed by @charliermarsh on 2023-07-12 21:08_

---

_Branch deleted on 2023-07-12 21:08_

---
