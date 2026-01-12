```yaml
number: 5520
title: "Avoid triggering `unnecessary-map` (`C417`) for late-bound lambdas"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/late-binding
created_at: 2023-07-05T01:46:25Z
updated_at: 2023-07-05T02:26:00Z
url: https://github.com/astral-sh/ruff/pull/5520
synced_at: 2026-01-12T03:36:55Z
```

# Avoid triggering `unnecessary-map` (`C417`) for late-bound lambdas

---

_Pull request opened by @charliermarsh on 2023-07-05 01:46_

Closes https://github.com/astral-sh/ruff/issues/5502.

---

_Label `bug` added by @charliermarsh on 2023-07-05 01:46_

---

_Comment by @github-actions[bot] on 2023-07-05 02:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     10.6±0.35ms     3.9 MB/sec    1.00     10.2±0.36ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.3±0.13ms     7.2 MB/sec    1.00      2.2±0.09ms     7.5 MB/sec
formatter/numpy/globals.py                 1.01   262.6±18.09µs    11.2 MB/sec    1.00   260.3±14.67µs    11.3 MB/sec
formatter/pydantic/types.py                1.04      5.1±0.19ms     5.0 MB/sec    1.00      4.9±0.21ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.05     18.7±0.62ms     2.2 MB/sec    1.00     17.8±0.36ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.13ms     3.8 MB/sec    1.00      4.3±0.15ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   571.9±23.47µs     5.2 MB/sec    1.00   574.5±24.56µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.0±0.28ms     3.2 MB/sec    1.00      7.8±0.21ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.04      9.1±0.30ms     4.5 MB/sec    1.00      8.8±0.22ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1935.9±63.41µs     8.6 MB/sec    1.00  1882.4±59.19µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.01   230.1±11.64µs    12.8 MB/sec    1.00   228.4±10.32µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.1±0.15ms     6.2 MB/sec    1.00      4.0±0.14ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00     10.8±0.49ms     3.8 MB/sec     1.00     10.7±0.52ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.10ms     7.4 MB/sec     1.02      2.3±0.13ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00   262.6±14.15µs    11.2 MB/sec     1.02   268.9±23.46µs    11.0 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.24ms     5.1 MB/sec     1.03      5.2±0.30ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     17.5±0.58ms     2.3 MB/sec     1.00     17.4±0.54ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.21ms     3.6 MB/sec     1.01      4.7±0.28ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   566.0±21.71µs     5.2 MB/sec     1.01   571.2±33.39µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.26ms     3.2 MB/sec     1.02      8.0±0.34ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.1±0.42ms     4.5 MB/sec     1.01      9.3±0.36ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1962.6±136.67µs     8.5 MB/sec    1.02      2.0±0.08ms     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   242.4±13.75µs    12.2 MB/sec     1.01   244.3±14.86µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.14ms     6.2 MB/sec     1.01      4.1±0.17ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-05 02:11_

---

_Closed by @charliermarsh on 2023-07-05 02:11_

---

_Branch deleted on 2023-07-05 02:11_

---
