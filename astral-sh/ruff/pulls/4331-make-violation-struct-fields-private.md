```yaml
number: 4331
title: Make violation struct fields private
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/private
created_at: 2023-05-09T21:37:11Z
updated_at: 2023-05-09T22:22:42Z
url: https://github.com/astral-sh/ruff/pull/4331
synced_at: 2026-01-12T15:55:15Z
```

# Make violation struct fields private

---

_@charliermarsh_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-05-09 21:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     12.5±0.16ms     3.2 MB/sec    1.00     12.5±0.07ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.03ms     5.6 MB/sec    1.00      3.0±0.01ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.8±0.78µs     8.1 MB/sec    1.01    368.9±0.56µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.02ms     5.0 MB/sec    1.01      5.1±0.03ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.1±0.02ms     6.7 MB/sec    1.01      6.1±0.03ms     6.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1304.7±4.42µs    12.8 MB/sec    1.00   1307.9±7.10µs    12.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    142.6±0.19µs    20.7 MB/sec    1.02    145.3±5.54µs    20.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.7±0.00ms     9.4 MB/sec    1.01      2.7±0.01ms     9.3 MB/sec
parser/large/dataset.py                    1.00      4.8±0.01ms     8.4 MB/sec    1.00      4.8±0.01ms     8.5 MB/sec
parser/numpy/ctypeslib.py                  1.00    939.9±0.59µs    17.7 MB/sec    1.00    940.4±0.91µs    17.7 MB/sec
parser/numpy/globals.py                    1.00     95.2±0.21µs    31.0 MB/sec    1.00     95.2±0.17µs    31.0 MB/sec
parser/pydantic/types.py                   1.00      2.1±0.00ms    12.4 MB/sec    1.00      2.1±0.00ms    12.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.2±0.49ms     2.0 MB/sec    1.08     21.8±0.41ms  1913.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.19ms     3.3 MB/sec    1.04      5.3±0.21ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   590.9±21.15µs     5.0 MB/sec    1.04   616.5±20.80µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.22ms     3.0 MB/sec    1.07      9.1±0.26ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.19ms     4.1 MB/sec    1.19     11.8±0.36ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.06ms     7.8 MB/sec    1.14      2.4±0.08ms     6.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   237.6±26.45µs    12.4 MB/sec    1.11   263.3±10.93µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.12ms     5.7 MB/sec    1.16      5.2±0.15ms     4.9 MB/sec
parser/large/dataset.py                    1.00      8.0±0.13ms     5.1 MB/sec    1.02      8.2±0.22ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1548.9±51.41µs    10.8 MB/sec    1.01  1567.1±50.95µs    10.6 MB/sec
parser/numpy/globals.py                    1.01    156.5±4.97µs    18.9 MB/sec    1.00    155.7±5.12µs    19.0 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.08ms     7.5 MB/sec    1.00      3.4±0.07ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-09 22:00_

---

_Closed by @charliermarsh on 2023-05-09 22:00_

---

_Branch deleted on 2023-05-09 22:00_

---
