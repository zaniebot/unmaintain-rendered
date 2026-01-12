```yaml
number: 5867
title: "Use `relativize_path` for `noqa` warnings"
type: pull_request
state: merged
author: sobolevn
labels: []
assignees: []
merged: true
base: main
head: patch-4
created_at: 2023-07-18T16:28:37Z
updated_at: 2023-07-18T17:06:28Z
url: https://github.com/astral-sh/ruff/pull/5867
synced_at: 2026-01-12T03:30:22Z
```

# Use `relativize_path` for `noqa` warnings

---

_Pull request opened by @sobolevn on 2023-07-18 16:28_

Refs https://github.com/astral-sh/ruff/pull/5856


---

_Comment by @charliermarsh on 2023-07-18 16:44_

Thanks!

---

_Merged by @charliermarsh on 2023-07-18 16:44_

---

_Closed by @charliermarsh on 2023-07-18 16:44_

---

_Comment by @github-actions[bot] on 2023-07-18 16:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.6±0.37ms     3.8 MB/sec  
formatter/numpy/ctypeslib.py               1.00      2.1±0.09ms     7.8 MB/sec  
formatter/numpy/globals.py                 1.00   252.0±12.07µs    11.7 MB/sec  
formatter/pydantic/types.py                1.00      4.6±0.15ms     5.6 MB/sec  
linter/all-rules/large/dataset.py          1.00     14.7±0.66ms     2.8 MB/sec    1.00     14.6±0.82ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.7±0.21ms     4.5 MB/sec    1.00      3.6±0.13ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.07   531.3±20.75µs     5.6 MB/sec    1.00   495.8±21.19µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.09      7.0±0.22ms     3.6 MB/sec    1.00      6.5±0.26ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.09      7.8±0.22ms     5.2 MB/sec    1.00      7.2±0.24ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.08  1679.7±52.97µs     9.9 MB/sec    1.00  1553.3±67.63µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.07   195.7±11.26µs    15.1 MB/sec    1.00   183.4±11.20µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.08      3.6±0.12ms     7.2 MB/sec    1.00      3.3±0.15ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.3±0.13ms     3.6 MB/sec    1.01     11.4±0.16ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.6 MB/sec    1.00      2.2±0.03ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    247.1±4.64µs    11.9 MB/sec    1.04   257.2±18.15µs    11.5 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.08ms     5.3 MB/sec    1.00      4.8±0.06ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.15ms     2.6 MB/sec    1.00     16.0±0.22ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.0 MB/sec    1.00      4.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.5±7.48µs     5.9 MB/sec    1.01   501.3±10.29µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.09ms     3.5 MB/sec    1.00      7.2±0.12ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.09ms     5.0 MB/sec    1.00      8.2±0.10ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1690.5±21.84µs     9.8 MB/sec    1.01  1700.9±20.85µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.3±3.69µs    15.4 MB/sec    1.00    191.9±3.64µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.0 MB/sec    1.01      3.7±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
