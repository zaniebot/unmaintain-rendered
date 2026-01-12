```yaml
number: 4292
title: Clarify some docstring-related docs
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/docstring-docs
created_at: 2023-05-08T22:19:23Z
updated_at: 2023-05-08T22:47:33Z
url: https://github.com/astral-sh/ruff/pull/4292
synced_at: 2026-01-12T03:56:39Z
```

# Clarify some docstring-related docs

---

_Pull request opened by @charliermarsh on 2023-05-08 22:19_

Closes #4105.

---

_Label `documentation` added by @charliermarsh on 2023-05-08 22:19_

---

_Marked ready for review by @charliermarsh on 2023-05-08 22:19_

---

_Merged by @charliermarsh on 2023-05-08 22:24_

---

_Closed by @charliermarsh on 2023-05-08 22:24_

---

_Branch deleted on 2023-05-08 22:24_

---

_Comment by @github-actions[bot] on 2023-05-08 22:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     16.8±0.13ms     2.4 MB/sec    1.00     16.4±0.21ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.0±0.01ms     4.2 MB/sec    1.00      3.9±0.07ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.02    494.2±1.19µs     6.0 MB/sec    1.00    484.4±4.60µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.02ms     3.6 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.03      8.3±0.05ms     4.9 MB/sec    1.00      8.1±0.09ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1796.9±31.06µs     9.3 MB/sec    1.00  1736.3±33.27µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.7±0.71µs    15.1 MB/sec    1.00    195.5±3.06µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.7±0.01ms     6.9 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
parser/large/dataset.py                    1.02      6.6±0.02ms     6.2 MB/sec    1.00      6.5±0.02ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.01   1278.3±1.59µs    13.0 MB/sec    1.00  1268.9±10.08µs    13.1 MB/sec
parser/numpy/globals.py                    1.01    129.3±0.84µs    22.8 MB/sec    1.00    127.6±1.56µs    23.1 MB/sec
parser/pydantic/types.py                   1.02      2.8±0.00ms     9.1 MB/sec    1.00      2.7±0.02ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.19ms     2.4 MB/sec    1.02     17.1±0.36ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     3.9 MB/sec    1.00      4.3±0.05ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.4±6.77µs     6.9 MB/sec    1.02    438.3±8.38µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.08ms     3.6 MB/sec    1.01      7.1±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.15ms     4.8 MB/sec    1.00      8.6±0.05ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1826.3±46.87µs     9.1 MB/sec    1.00  1799.0±19.72µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.2±4.00µs    15.3 MB/sec    1.01    193.5±5.03µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.06ms     6.6 MB/sec    1.00      3.9±0.04ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.8±0.03ms     6.0 MB/sec    1.01      6.9±0.12ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1309.9±14.31µs    12.7 MB/sec    1.00  1308.3±18.96µs    12.7 MB/sec
parser/numpy/globals.py                    1.01    135.1±1.54µs    21.8 MB/sec    1.00    133.9±1.76µs    22.0 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.8 MB/sec    1.00      2.9±0.02ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
