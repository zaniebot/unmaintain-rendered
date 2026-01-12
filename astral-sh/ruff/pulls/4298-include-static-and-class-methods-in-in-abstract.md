```yaml
number: 4298
title: Include static and class methods in in abstract decorator list
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/abc
created_at: 2023-05-09T01:48:58Z
updated_at: 2023-05-09T02:14:53Z
url: https://github.com/astral-sh/ruff/pull/4298
synced_at: 2026-01-12T15:55:15Z
```

# Include static and class methods in in abstract decorator list

---

_@charliermarsh_

Closes #4285.

---

_Merged by @charliermarsh on 2023-05-09 01:54_

---

_Closed by @charliermarsh on 2023-05-09 01:54_

---

_Branch deleted on 2023-05-09 01:54_

---

_Comment by @github-actions[bot] on 2023-05-09 01:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.14ms     2.9 MB/sec    1.08     15.3±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     5.0 MB/sec    1.05      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    416.0±1.09µs     7.1 MB/sec    1.02    425.7±2.19µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.06ms     4.4 MB/sec    1.07      6.2±0.07ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.04ms     5.9 MB/sec    1.12      7.8±0.06ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1477.1±3.53µs    11.3 MB/sec    1.09   1609.4±2.58µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.7±0.26µs    18.2 MB/sec    1.07    172.5±0.26µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.10      3.4±0.02ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.5±0.01ms     7.5 MB/sec    1.00      5.5±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1059.4±1.09µs    15.7 MB/sec    1.01   1066.1±0.93µs    15.6 MB/sec
parser/numpy/globals.py                    1.00    107.3±0.16µs    27.5 MB/sec    1.00    107.4±0.31µs    27.5 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.01      2.3±0.00ms    10.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.5±0.04ms     3.0 MB/sec    1.00     13.4±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    341.8±2.73µs     8.6 MB/sec    1.00    343.3±2.47µs     8.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.6±0.04ms     4.5 MB/sec    1.00      5.6±0.07ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.02ms     5.8 MB/sec    1.00      7.0±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1430.3±5.77µs    11.6 MB/sec    1.00   1427.5±5.75µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    152.6±1.71µs    19.3 MB/sec    1.01    153.6±2.85µs    19.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.3 MB/sec    1.00      3.1±0.02ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.7±0.01ms     7.1 MB/sec    1.00      5.7±0.02ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1070.1±4.15µs    15.6 MB/sec    1.00   1065.8±5.71µs    15.6 MB/sec
parser/numpy/globals.py                    1.00    106.7±0.74µs    27.7 MB/sec    1.00    107.0±0.95µs    27.6 MB/sec
parser/pydantic/types.py                   1.01      2.4±0.04ms    10.6 MB/sec    1.00      2.4±0.01ms    10.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
