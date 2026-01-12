```yaml
number: 5103
title: "Remove manual `await` detection"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/await
created_at: 2023-06-14T22:05:41Z
updated_at: 2023-06-14T22:35:55Z
url: https://github.com/astral-sh/ruff/pull/5103
synced_at: 2026-01-12T15:55:17Z
```

# Remove manual `await` detection

---

_@charliermarsh_

We can just use `any_over_expr` instead.

---

_Label `internal` added by @charliermarsh on 2023-06-14 22:05_

---

_Merged by @charliermarsh on 2023-06-14 22:17_

---

_Closed by @charliermarsh on 2023-06-14 22:17_

---

_Branch deleted on 2023-06-14 22:17_

---

_Comment by @github-actions[bot] on 2023-06-14 22:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.5±0.34ms     5.4 MB/sec    1.03      7.7±0.37ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1610.0±59.76µs    10.3 MB/sec    1.02  1639.2±93.08µs    10.2 MB/sec
formatter/numpy/globals.py                 1.05   162.4±10.99µs    18.2 MB/sec    1.00    155.4±9.46µs    19.0 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.13ms     8.3 MB/sec    1.03      3.2±0.18ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.62ms     2.6 MB/sec    1.08     17.3±0.63ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.14ms     4.3 MB/sec    1.04      4.0±0.15ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   481.6±25.56µs     6.1 MB/sec    1.12   538.0±28.61µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.34ms     3.5 MB/sec    1.00      7.3±0.25ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.28ms     5.1 MB/sec    1.04      8.3±0.32ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1712.3±57.42µs     9.7 MB/sec    1.02  1744.2±78.46µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   214.9±29.16µs    13.7 MB/sec    1.00   214.7±18.71µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.14ms     6.8 MB/sec    1.00      3.7±0.19ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.13ms     5.1 MB/sec    1.00      7.9±0.05ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1594.9±15.84µs    10.4 MB/sec    1.01  1610.7±30.07µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    158.2±5.76µs    18.7 MB/sec    1.00    157.9±3.28µs    18.7 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.02ms     7.9 MB/sec    1.00      3.3±0.05ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.00     16.7±0.16ms     2.4 MB/sec    1.01     16.9±0.11ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.03ms     3.9 MB/sec    1.01      4.3±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   445.5±16.23µs     6.6 MB/sec    1.00   443.7±13.32µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.09ms     3.5 MB/sec    1.00      7.4±0.09ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.04ms     4.8 MB/sec    1.00      8.5±0.05ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1783.1±20.35µs     9.3 MB/sec    1.00  1780.9±23.22µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.2±1.53µs    15.5 MB/sec    1.01    191.3±4.69µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.07ms     6.6 MB/sec    1.00      3.9±0.06ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
