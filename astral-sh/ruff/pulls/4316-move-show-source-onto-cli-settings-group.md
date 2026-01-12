```yaml
number: 4316
title: "Move `show_source` onto CLI settings group"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: charlie/update-check
head: charlie/show-source
created_at: 2023-05-09T17:01:38Z
updated_at: 2023-05-09T17:40:17Z
url: https://github.com/astral-sh/ruff/pull/4316
synced_at: 2026-01-12T15:55:15Z
```

# Move `show_source` onto CLI settings group

---

_@charliermarsh_

Now that we _always_ compute source, we don't need it available as a library setting -- it's only relevant for the printer.

---

_Merged by @charliermarsh on 2023-05-09 17:01_

---

_Closed by @charliermarsh on 2023-05-09 17:01_

---

_Branch deleted on 2023-05-09 17:01_

---

_Branch restored on 2023-05-09 17:01_

---

_Comment by @charliermarsh on 2023-05-09 17:03_

Accidentally merged into `charlie/update-check`, re-doing.

---

_Comment by @github-actions[bot] on 2023-05-09 17:40_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.10     19.1±0.73ms     2.1 MB/sec    1.00     17.3±0.70ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      4.3±0.17ms     3.8 MB/sec    1.00      4.1±0.22ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   525.9±26.84µs     5.6 MB/sec    1.00   517.7±22.77µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.08      7.7±0.26ms     3.3 MB/sec    1.00      7.2±0.38ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.6±0.23ms     4.8 MB/sec    1.00      8.4±0.37ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1831.4±42.91µs     9.1 MB/sec    1.00  1760.3±63.10µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    211.2±9.09µs    14.0 MB/sec    1.01    212.9±9.84µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.12ms     6.9 MB/sec    1.00      3.7±0.15ms     6.9 MB/sec
parser/large/dataset.py                    1.02      6.8±0.12ms     6.0 MB/sec    1.00      6.7±0.26ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.02  1331.2±54.28µs    12.5 MB/sec    1.00  1303.1±43.58µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    133.2±4.70µs    22.1 MB/sec    1.01    135.2±7.93µs    21.8 MB/sec
parser/pydantic/types.py                   1.01      3.0±0.10ms     8.6 MB/sec    1.00      2.9±0.20ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.4±0.98ms  1945.6 KB/sec    1.02     21.9±0.87ms  1904.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.18ms     3.1 MB/sec    1.05      5.6±0.25ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   630.9±24.06µs     4.7 MB/sec    1.03   647.9±50.45µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.31ms     2.9 MB/sec    1.05      9.3±0.44ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     11.2±0.57ms     3.6 MB/sec    1.00     11.1±0.36ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.15ms     6.9 MB/sec    1.01      2.4±0.11ms     6.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   263.3±12.34µs    11.2 MB/sec    1.08   285.0±24.67µs    10.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.25ms     5.2 MB/sec    1.00      4.9±0.50ms     5.2 MB/sec
parser/large/dataset.py                    1.00      8.7±0.28ms     4.7 MB/sec    1.00      8.7±0.23ms     4.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1699.3±68.21µs     9.8 MB/sec    1.02  1733.7±67.31µs     9.6 MB/sec
parser/numpy/globals.py                    1.00    175.4±9.88µs    16.8 MB/sec    1.00    174.8±7.70µs    16.9 MB/sec
parser/pydantic/types.py                   1.01      3.9±0.16ms     6.5 MB/sec    1.00      3.9±0.19ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
