```yaml
number: 4312
title: "Update `mkdocs` unformatted example error message"
type: pull_request
state: merged
author: calumy
labels:
  - documentation
assignees: []
merged: true
base: main
head: update-docs-error
created_at: 2023-05-09T16:28:04Z
updated_at: 2023-05-10T14:55:25Z
url: https://github.com/astral-sh/ruff/pull/4312
synced_at: 2026-01-12T15:55:15Z
```

# Update `mkdocs` unformatted example error message

---

_@calumy_

In #3979 it became clear that the error message could be clearer that only the example section of the documentation should be updated. This PR clarifies this.

---

_Renamed from "Update mkdocs unformaged example error message" to "Update `mkdocs` unformatted example error message" by @charliermarsh on 2023-05-09 16:36_

---

_Merged by @charliermarsh on 2023-05-09 16:36_

---

_Closed by @charliermarsh on 2023-05-09 16:36_

---

_Label `documentation` added by @charliermarsh on 2023-05-09 16:36_

---

_Comment by @github-actions[bot] on 2023-05-09 16:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.9±0.03ms     2.9 MB/sec    1.00     13.8±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    413.4±0.74µs     7.1 MB/sec    1.00    411.5±0.68µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.9±0.01ms     5.9 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1485.9±2.11µs    11.2 MB/sec    1.00   1475.5±1.95µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    163.1±0.23µs    18.1 MB/sec    1.00    161.9±0.14µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.00ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.01      5.5±0.00ms     7.5 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.01   1063.8±0.71µs    15.7 MB/sec    1.00   1057.2±2.00µs    15.8 MB/sec
parser/numpy/globals.py                    1.00    108.1±0.15µs    27.3 MB/sec    1.00    107.8±0.16µs    27.4 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     23.9±1.02ms  1740.9 KB/sec    1.00     23.6±0.73ms  1764.8 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.8±0.18ms     2.9 MB/sec    1.00      5.8±0.21ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   671.1±29.49µs     4.4 MB/sec    1.02   681.7±37.74µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.8±0.36ms     2.6 MB/sec    1.02     10.0±0.42ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     11.7±0.42ms     3.5 MB/sec    1.01     11.8±0.34ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.5±0.10ms     6.7 MB/sec    1.01      2.5±0.12ms     6.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   284.1±12.60µs    10.4 MB/sec    1.03   293.1±16.98µs    10.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      5.3±0.19ms     4.9 MB/sec    1.00      5.2±0.19ms     4.9 MB/sec
parser/large/dataset.py                    1.00      9.6±0.31ms     4.2 MB/sec    1.01      9.7±0.29ms     4.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1829.7±58.80µs     9.1 MB/sec    1.00  1836.0±53.77µs     9.1 MB/sec
parser/numpy/globals.py                    1.00   186.7±12.08µs    15.8 MB/sec    1.00    187.0±8.61µs    15.8 MB/sec
parser/pydantic/types.py                   1.01      4.1±0.18ms     6.1 MB/sec    1.00      4.1±0.28ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-05-10 14:55_

---
