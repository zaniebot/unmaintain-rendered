```yaml
number: 5507
title: Update documentation to list double-quote preference first
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/quotes
created_at: 2023-07-04T17:58:44Z
updated_at: 2023-07-04T18:28:58Z
url: https://github.com/astral-sh/ruff/pull/5507
synced_at: 2026-01-12T03:36:55Z
```

# Update documentation to list double-quote preference first

---

_Pull request opened by @charliermarsh on 2023-07-04 17:58_

Closes https://github.com/astral-sh/ruff/issues/5496.

---

_Label `documentation` added by @charliermarsh on 2023-07-04 17:58_

---

_Merged by @charliermarsh on 2023-07-04 18:06_

---

_Closed by @charliermarsh on 2023-07-04 18:06_

---

_Branch deleted on 2023-07-04 18:06_

---

_Comment by @github-actions[bot] on 2023-07-04 18:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.27ms     5.1 MB/sec    1.06      8.5±0.57ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1774.0±65.50µs     9.4 MB/sec    1.13      2.0±0.06ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00   206.6±11.10µs    14.3 MB/sec    1.01   208.2±13.03µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.13ms     6.5 MB/sec    1.06      4.2±0.45ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.49ms     2.9 MB/sec    1.07     14.9±0.46ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.13ms     4.8 MB/sec    1.06      3.7±0.14ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   455.9±20.01µs     6.5 MB/sec    1.01   462.0±19.55µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.30ms     4.1 MB/sec    1.03      6.5±0.24ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.21ms     5.8 MB/sec    1.14      8.0±0.22ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1539.1±58.98µs    10.8 MB/sec    1.08  1669.4±55.72µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.02   188.4±23.51µs    15.7 MB/sec    1.00    184.6±8.36µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.13ms     8.0 MB/sec    1.14      3.6±0.16ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.11     11.7±0.33ms     3.5 MB/sec    1.00     10.6±0.35ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.07      2.6±0.11ms     6.5 MB/sec    1.00      2.4±0.12ms     6.9 MB/sec
formatter/numpy/globals.py                 1.03   283.0±12.09µs    10.4 MB/sec    1.00   275.0±21.99µs    10.7 MB/sec
formatter/pydantic/types.py                1.06      5.5±0.21ms     4.6 MB/sec    1.00      5.2±0.21ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.06     19.7±0.41ms     2.1 MB/sec    1.00     18.5±0.83ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.1±0.15ms     3.3 MB/sec    1.00      4.9±0.27ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.03   630.5±34.21µs     4.7 MB/sec    1.00   611.9±39.57µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.06      8.8±0.30ms     2.9 MB/sec    1.00      8.3±0.31ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.04     10.1±0.30ms     4.0 MB/sec    1.00      9.7±0.42ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06      2.2±0.07ms     7.7 MB/sec    1.00      2.0±0.08ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.08    257.4±8.33µs    11.5 MB/sec    1.00   237.6±10.64µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.06      4.6±0.14ms     5.6 MB/sec    1.00      4.3±0.13ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
