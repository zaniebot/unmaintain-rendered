```yaml
number: 5579
title: "Bugfix: Remove version numbers from pypi links"
type: pull_request
state: merged
author: petermattia
labels: []
assignees: []
merged: true
base: main
head: update-pypi-links
created_at: 2023-07-07T05:55:17Z
updated_at: 2023-07-07T15:52:27Z
url: https://github.com/astral-sh/ruff/pull/5579
synced_at: 2026-01-12T15:55:18Z
```

# Bugfix: Remove version numbers from pypi links

---

_@petermattia_

## Summary

There are two pypi links in the documentation that link to specific version numbers of other packages. Removing these versioned links allows users to immediately view the latest version of the package and maintains consistency with the other links.

## Test Plan

N/A


---

_Comment by @github-actions[bot] on 2023-07-07 06:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.06ms     4.3 MB/sec    1.00      9.4±0.02ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.01ms     7.9 MB/sec    1.00      2.1±0.01ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00    234.0±2.28µs    12.6 MB/sec    1.03   240.4±16.94µs    12.3 MB/sec
formatter/pydantic/types.py                1.01      4.6±0.01ms     5.6 MB/sec    1.00      4.5±0.01ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     16.1±0.07ms     2.5 MB/sec    1.00     16.1±0.05ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.1 MB/sec    1.00      4.0±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    516.1±1.45µs     5.7 MB/sec    1.00    517.9±0.92µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.03ms     3.6 MB/sec    1.00      7.1±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.02ms     5.1 MB/sec    1.00      8.0±0.02ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1760.5±6.29µs     9.5 MB/sec    1.00   1752.4±5.84µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.1±0.67µs    14.5 MB/sec    1.00    203.8±0.34µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.05ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06     11.8±0.30ms     3.5 MB/sec    1.00     11.1±0.33ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.5±0.08ms     6.6 MB/sec    1.00      2.4±0.08ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00    272.5±9.65µs    10.8 MB/sec    1.00   273.8±16.79µs    10.8 MB/sec
formatter/pydantic/types.py                1.00      5.4±0.15ms     4.7 MB/sec    1.01      5.4±0.26ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.01     19.0±0.34ms     2.1 MB/sec    1.00     18.8±0.43ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.9±0.11ms     3.4 MB/sec    1.00      4.8±0.14ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   586.9±20.24µs     5.0 MB/sec    1.01   594.5±18.24µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.21ms     3.1 MB/sec    1.01      8.4±0.21ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.01      9.7±0.22ms     4.2 MB/sec    1.00      9.6±0.18ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.06ms     8.3 MB/sec    1.02      2.1±0.05ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    234.1±8.89µs    12.6 MB/sec    1.00   232.4±12.56µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.13ms     6.0 MB/sec    1.00      4.2±0.14ms     6.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-07 08:52_

---

_Merged by @charliermarsh on 2023-07-07 13:35_

---

_Closed by @charliermarsh on 2023-07-07 13:35_

---

_Branch deleted on 2023-07-07 15:52_

---
