```yaml
number: 4232
title: "Update doc defaults for `section-order`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/section-order
created_at: 2023-05-04T21:29:17Z
updated_at: 2023-05-04T22:02:20Z
url: https://github.com/astral-sh/ruff/pull/4232
synced_at: 2026-01-12T04:28:19Z
```

# Update doc defaults for `section-order`

---

_Pull request opened by @charliermarsh on 2023-05-04 21:29_

Closes #4231.

---

_Merged by @charliermarsh on 2023-05-04 21:35_

---

_Closed by @charliermarsh on 2023-05-04 21:35_

---

_Branch deleted on 2023-05-04 21:35_

---

_Comment by @github-actions[bot] on 2023-05-04 21:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.5±0.13ms     2.8 MB/sec    1.00     14.3±0.14ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.03ms     4.9 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    420.1±1.46µs     7.0 MB/sec    1.00    419.2±1.17µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.09ms     4.3 MB/sec    1.00      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.04ms     5.8 MB/sec    1.00      7.0±0.04ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1493.7±2.23µs    11.1 MB/sec    1.00   1492.2±3.35µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.0±0.32µs    17.6 MB/sec    1.00    167.9±0.84µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.5±0.01ms     7.4 MB/sec    1.00      5.5±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1061.5±1.46µs    15.7 MB/sec    1.00   1057.1±2.21µs    15.8 MB/sec
parser/numpy/globals.py                    1.00    108.4±0.48µs    27.2 MB/sec    1.00    108.0±0.36µs    27.3 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.01ms    11.0 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     16.5±0.21ms     2.5 MB/sec    1.00     16.2±0.12ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.05ms     4.0 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    492.9±6.72µs     6.0 MB/sec    1.00    487.0±7.22µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.10ms     3.6 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.4±0.05ms     4.8 MB/sec    1.00      8.2±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1786.6±20.83µs     9.3 MB/sec    1.00  1745.4±13.58µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    199.5±3.12µs    14.8 MB/sec    1.00    197.6±4.71µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.03ms     6.7 MB/sec    1.00      3.7±0.05ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.7±0.04ms     6.1 MB/sec    1.00      6.6±0.03ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1269.8±14.32µs    13.1 MB/sec    1.00  1268.8±11.64µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    131.4±2.26µs    22.5 MB/sec    1.00    130.9±2.17µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.0 MB/sec    1.00      2.8±0.04ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
