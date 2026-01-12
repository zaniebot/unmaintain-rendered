```yaml
number: 4712
title: "Fix docs formatting for `iter-method-returns-iterable`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/fix-docs
created_at: 2023-05-29T21:27:53Z
updated_at: 2023-05-29T21:55:24Z
url: https://github.com/astral-sh/ruff/pull/4712
synced_at: 2026-01-12T15:55:16Z
```

# Fix docs formatting for `iter-method-returns-iterable`

---

_@charliermarsh_

(My mistake.)

---

_Renamed from "Fix docs formatting for iter-method-returns-iterable" to "Fix docs formatting for `iter-method-returns-iterable`" by @charliermarsh on 2023-05-29 21:28_

---

_Label `documentation` added by @charliermarsh on 2023-05-29 21:28_

---

_Merged by @charliermarsh on 2023-05-29 21:34_

---

_Closed by @charliermarsh on 2023-05-29 21:34_

---

_Branch deleted on 2023-05-29 21:34_

---

_Comment by @github-actions[bot] on 2023-05-29 21:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.10ms     2.7 MB/sec    1.01     15.2±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.6 MB/sec    1.01      3.7±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    374.2±1.15µs     7.9 MB/sec    1.01    377.0±0.94µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.01      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.02      7.6±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1577.3±4.01µs    10.6 MB/sec    1.02   1605.0±5.18µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.8±0.21µs    17.1 MB/sec    1.02    175.7±1.07µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.02      3.4±0.04ms     7.5 MB/sec
parser/large/dataset.py                    1.01      5.8±0.01ms     7.0 MB/sec    1.00      5.8±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.01   1135.8±2.58µs    14.7 MB/sec    1.00   1130.1±1.69µs    14.7 MB/sec
parser/numpy/globals.py                    1.00    115.5±0.27µs    25.6 MB/sec    1.00    115.8±0.24µs    25.5 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.7±0.44ms     2.1 MB/sec    1.07     21.1±0.63ms  1977.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.34ms     3.1 MB/sec    1.03      5.4±0.39ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.04   615.8±45.46µs     4.8 MB/sec    1.00   592.7±32.81µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.28ms     3.0 MB/sec    1.05      9.0±0.39ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.03     10.2±0.55ms     4.0 MB/sec    1.00      9.9±0.49ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.08ms     7.8 MB/sec    1.00      2.1±0.08ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.03   256.4±15.43µs    11.5 MB/sec    1.00    248.8±9.16µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.14ms     5.7 MB/sec    1.00      4.5±0.16ms     5.7 MB/sec
parser/large/dataset.py                    1.00      7.8±0.17ms     5.2 MB/sec    1.02      8.0±0.22ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1498.3±41.50µs    11.1 MB/sec    1.01  1514.1±56.93µs    11.0 MB/sec
parser/numpy/globals.py                    1.00    153.7±8.21µs    19.2 MB/sec    1.02   157.4±13.67µs    18.7 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.10ms     7.6 MB/sec    1.00      3.4±0.09ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
