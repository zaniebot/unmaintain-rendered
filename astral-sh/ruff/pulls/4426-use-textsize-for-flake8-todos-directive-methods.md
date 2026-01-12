```yaml
number: 4426
title: "Use `TextSize` for flake8-todos `Directive` methods"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/comment
created_at: 2023-05-14T01:59:16Z
updated_at: 2023-05-14T02:26:53Z
url: https://github.com/astral-sh/ruff/pull/4426
synced_at: 2026-01-12T15:55:15Z
```

# Use `TextSize` for flake8-todos `Directive` methods

---

_@charliermarsh_

Some follow-ups from #4413.

---

_Renamed from "Use TextSize for flake8-todos Directive methods" to "Use `TextSize` for flake8-todos `Directive` methods" by @charliermarsh on 2023-05-14 02:04_

---

_Merged by @charliermarsh on 2023-05-14 02:05_

---

_Closed by @charliermarsh on 2023-05-14 02:05_

---

_Branch deleted on 2023-05-14 02:05_

---

_Comment by @github-actions[bot] on 2023-05-14 02:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.7±0.01ms     3.0 MB/sec    1.00     13.7±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    410.6±0.85µs     7.2 MB/sec    1.00    410.7±1.26µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.01ms     4.5 MB/sec    1.00      5.7±0.01ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.1 MB/sec    1.01      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1427.8±3.01µs    11.7 MB/sec    1.01   1436.1±4.10µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.0±0.45µs    18.6 MB/sec    1.01    159.9±0.56µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.5 MB/sec    1.00      5.4±0.01ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.02   1060.8±0.58µs    15.7 MB/sec    1.00   1045.1±0.60µs    15.9 MB/sec
parser/numpy/globals.py                    1.00    106.1±0.20µs    27.8 MB/sec    1.00    106.0±0.20µs    27.8 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.03     24.0±1.43ms  1733.7 KB/sec     1.00     23.4±1.33ms  1783.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.36ms     2.9 MB/sec     1.07      6.1±0.40ms     2.7 MB/sec
linter/all-rules/numpy/globals.py          1.07   667.9±44.02µs     4.4 MB/sec     1.00   625.9±24.91µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.08      9.7±0.59ms     2.6 MB/sec     1.00      9.0±0.39ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.02     10.9±0.66ms     3.7 MB/sec     1.00     10.7±0.33ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.3±0.13ms     7.4 MB/sec     1.00      2.2±0.11ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   265.0±13.61µs    11.1 MB/sec     1.00   266.1±15.44µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.19ms     5.4 MB/sec     1.00      4.7±0.16ms     5.4 MB/sec
parser/large/dataset.py                    1.02      8.9±0.26ms     4.6 MB/sec     1.00      8.7±0.31ms     4.7 MB/sec
parser/numpy/ctypeslib.py                  1.06  1775.9±137.95µs     9.4 MB/sec    1.00  1679.8±45.27µs     9.9 MB/sec
parser/numpy/globals.py                    1.02    180.3±8.75µs    16.4 MB/sec     1.00    176.4±6.91µs    16.7 MB/sec
parser/pydantic/types.py                   1.04      4.0±0.21ms     6.4 MB/sec     1.00      3.8±0.11ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
