```yaml
number: 3640
title: Avoid raising PEP 604 errors with forward-referenced members
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/string
created_at: 2023-03-21T03:40:48Z
updated_at: 2023-03-21T04:14:02Z
url: https://github.com/astral-sh/ruff/pull/3640
synced_at: 2026-01-12T04:39:45Z
```

# Avoid raising PEP 604 errors with forward-referenced members

---

_Pull request opened by @charliermarsh on 2023-03-21 03:40_

Small follow-up to #3593.

---

_Merged by @charliermarsh on 2023-03-21 03:49_

---

_Closed by @charliermarsh on 2023-03-21 03:49_

---

_Branch deleted on 2023-03-21 03:49_

---

_Comment by @github-actions[bot] on 2023-03-21 03:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.2±0.46ms     2.4 MB/sec    1.00     17.2±0.42ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.14ms     3.7 MB/sec    1.02      4.6±0.19ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   619.2±22.57µs     4.8 MB/sec    1.00   621.6±21.26µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.6±0.18ms     3.4 MB/sec    1.01      7.7±0.34ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.22ms     4.6 MB/sec    1.02      9.0±0.27ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1980.5±57.53µs     8.4 MB/sec    1.02      2.0±0.07ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    243.0±8.76µs    12.1 MB/sec    1.03   250.2±20.34µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.17ms     6.0 MB/sec    1.01      4.3±0.18ms     6.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.5±0.14ms     3.0 MB/sec    1.00     13.3±0.16ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.05ms     4.6 MB/sec    1.00      3.7±0.07ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01   475.7±12.36µs     6.2 MB/sec    1.00    471.4±7.72µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.09ms     4.3 MB/sec    1.00      5.9±0.08ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.02      7.4±0.08ms     5.5 MB/sec    1.00      7.3±0.08ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1633.8±17.61µs    10.2 MB/sec    1.00  1616.7±42.48µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.9±3.65µs    16.5 MB/sec    1.00    179.1±4.85µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.04ms     7.4 MB/sec    1.00      3.4±0.07ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
