```yaml
number: 4495
title: "Move triple-quoted string detection into `Indexer` method"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/triple-quoted
created_at: 2023-05-18T14:34:54Z
updated_at: 2023-05-18T15:09:02Z
url: https://github.com/astral-sh/ruff/pull/4495
synced_at: 2026-01-12T15:55:15Z
```

# Move triple-quoted string detection into `Indexer` method

---

_@charliermarsh_

## Summary

An analogous change to the advice that was given in #4487.

---

_Renamed from "Move triple-quoted string detection into Indexer method" to "Move triple-quoted string detection into `Indexer` method" by @charliermarsh on 2023-05-18 14:34_

---

_Merged by @charliermarsh on 2023-05-18 14:42_

---

_Closed by @charliermarsh on 2023-05-18 14:42_

---

_Branch deleted on 2023-05-18 14:42_

---

_Comment by @github-actions[bot] on 2023-05-18 14:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.03ms     2.7 MB/sec    1.00     14.9±0.02ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.00ms     4.6 MB/sec    1.00      3.7±0.00ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    374.7±0.70µs     7.9 MB/sec    1.01    377.7±1.12µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.01      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1552.3±12.00µs    10.7 MB/sec    1.00   1559.4±4.61µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.7±0.34µs    17.7 MB/sec    1.02    169.8±0.57µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.01      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.9±0.01ms     6.9 MB/sec    1.01      5.9±0.01ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1155.0±0.62µs    14.4 MB/sec    1.01   1163.6±0.67µs    14.3 MB/sec
parser/numpy/globals.py                    1.00    119.3±0.58µs    24.7 MB/sec    1.01    120.3±0.62µs    24.5 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.00ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.06     17.9±1.74ms     2.3 MB/sec     1.00     16.8±1.50ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±1.49ms     3.6 MB/sec     1.00      4.7±2.06ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.01   507.4±16.85µs     5.8 MB/sec     1.00   500.4±11.76µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.14ms     3.5 MB/sec     1.00      7.2±1.34ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.09      9.2±1.24ms     4.4 MB/sec     1.00      8.4±1.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06  1897.0±282.53µs     8.8 MB/sec    1.00  1791.0±351.87µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   213.2±57.01µs    13.8 MB/sec     1.07  229.1±130.58µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.9±0.06ms     6.5 MB/sec     1.00      3.9±0.86ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.9±0.73ms     5.9 MB/sec     1.15      7.9±2.29ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1304.5±171.86µs    12.8 MB/sec    1.12  1460.0±356.86µs    11.4 MB/sec
parser/numpy/globals.py                    1.00   136.1±25.28µs    21.7 MB/sec     1.01    137.5±2.36µs    21.5 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.28ms     8.8 MB/sec     1.10      3.2±0.60ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
