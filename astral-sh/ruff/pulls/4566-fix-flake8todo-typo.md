```yaml
number: 4566
title: Fix Flake8Todo typo
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-flake8todo-typo
created_at: 2023-05-21T20:15:45Z
updated_at: 2023-05-21T21:25:10Z
url: https://github.com/astral-sh/ruff/pull/4566
synced_at: 2026-01-12T15:55:15Z
```

# Fix Flake8Todo typo

---

_@JonathanPlasse_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-05-21 20:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.2±0.08ms     2.9 MB/sec    1.00     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.02ms     4.8 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.4±0.72µs     7.0 MB/sec    1.00    421.9±0.96µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.04ms     4.3 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.02ms     6.0 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1460.1±2.88µs    11.4 MB/sec    1.00   1447.2±3.62µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.5±0.59µs    18.3 MB/sec    1.00    160.9±0.24µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.6 MB/sec    1.01      5.4±0.02ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1052.0±0.43µs    15.8 MB/sec    1.01   1058.2±1.26µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    108.4±0.38µs    27.2 MB/sec    1.01    109.0±0.61µs    27.1 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.01      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.30ms     2.4 MB/sec    1.00     16.6±0.26ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.11ms     3.9 MB/sec    1.00      4.2±0.10ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    501.9±6.85µs     5.9 MB/sec    1.00    497.2±8.01µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.14ms     3.7 MB/sec    1.00      7.0±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.07ms     4.9 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1729.1±23.01µs     9.6 MB/sec    1.01  1746.0±37.27µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.1±5.68µs    15.1 MB/sec    1.01    198.2±8.55µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.01      3.7±0.07ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.7±0.07ms     6.0 MB/sec    1.01      6.8±0.11ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1279.9±17.36µs    13.0 MB/sec    1.02  1299.1±26.17µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    132.6±2.98µs    22.3 MB/sec    1.01    134.0±3.63µs    22.0 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.07ms     8.9 MB/sec    1.00      2.9±0.05ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-21 20:32_

---

_Closed by @charliermarsh on 2023-05-21 20:32_

---

_Branch deleted on 2023-05-21 21:25_

---
