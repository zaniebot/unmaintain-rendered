```yaml
number: 5217
title: Remove defaults from fixtures/pyproject.toml
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pyproject
created_at: 2023-06-20T16:21:23Z
updated_at: 2023-06-20T17:36:12Z
url: https://github.com/astral-sh/ruff/pull/5217
synced_at: 2026-01-12T15:55:18Z
```

# Remove defaults from fixtures/pyproject.toml

---

_@charliermarsh_

## Summary

These should be encoded in the tests themselves, rather than here. In fact, I think they're all unused?


---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-20 16:22_

---

_Review requested from @konstin by @charliermarsh on 2023-06-20 16:22_

---

_Merged by @charliermarsh on 2023-06-20 17:16_

---

_Closed by @charliermarsh on 2023-06-20 17:16_

---

_Branch deleted on 2023-06-20 17:16_

---

_Comment by @github-actions[bot] on 2023-06-20 17:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.4±0.01ms     6.4 MB/sec    1.00      6.4±0.01ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1347.4±1.72µs    12.4 MB/sec    1.00   1350.3±6.70µs    12.3 MB/sec
formatter/numpy/globals.py                 1.00    131.4±0.27µs    22.5 MB/sec    1.02    133.8±0.14µs    22.0 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.7 MB/sec    1.01      2.7±0.01ms     9.6 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.02ms     3.1 MB/sec    1.00     12.9±0.04ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.00ms     5.0 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.3±2.33µs     6.9 MB/sec    1.00    429.7±0.65µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.2 MB/sec    1.00      6.6±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1469.8±2.71µs    11.3 MB/sec    1.00   1464.4±2.93µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.6±0.24µs    17.7 MB/sec    1.00    166.0±1.27µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.28ms     4.4 MB/sec    1.07     10.0±0.36ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1882.6±50.79µs     8.8 MB/sec    1.09      2.1±0.11ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00   201.7±26.85µs    14.6 MB/sec    1.05   211.1±12.30µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.12ms     6.8 MB/sec    1.08      4.1±0.16ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     18.2±0.34ms     2.2 MB/sec    1.08     19.6±0.52ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.17ms     3.5 MB/sec    1.10      5.3±0.24ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   599.5±23.27µs     4.9 MB/sec    1.06   636.1±19.91µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.19ms     3.1 MB/sec    1.07      8.8±0.35ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.24ms     4.2 MB/sec    1.10     10.7±0.32ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.06ms     8.0 MB/sec    1.06      2.2±0.09ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    250.1±8.08µs    11.8 MB/sec    1.06    265.0±9.46µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.29ms     5.6 MB/sec    1.04      4.7±0.21ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
