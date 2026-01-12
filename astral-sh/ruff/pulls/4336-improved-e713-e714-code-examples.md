```yaml
number: 4336
title: "Improved E713 & E714 code examples"
type: pull_request
state: merged
author: trag1c
labels:
  - documentation
assignees: []
merged: true
base: main
head: improve-e714-doc
created_at: 2023-05-10T02:19:28Z
updated_at: 2023-05-10T02:48:09Z
url: https://github.com/astral-sh/ruff/pull/4336
synced_at: 2026-01-12T15:55:15Z
```

# Improved E713 & E714 code examples

---

_@trag1c_

These felt *reaaally* weird seeing how the before/after code is completely changed. The weirdest part for me was E714 changing `not x is y` to `not (x in y)` 💀

---

_Merged by @charliermarsh on 2023-05-10 02:27_

---

_Closed by @charliermarsh on 2023-05-10 02:27_

---

_Label `documentation` added by @charliermarsh on 2023-05-10 02:27_

---

_Comment by @charliermarsh on 2023-05-10 02:28_

Thanks! These might've come from pycodestyle originally in some way.

---

_Comment by @github-actions[bot] on 2023-05-10 02:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.5±0.25ms     2.6 MB/sec    1.00     15.5±0.24ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.06ms     4.5 MB/sec    1.01      3.8±0.08ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    462.7±9.83µs     6.4 MB/sec    1.00   461.8±10.86µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.6±0.14ms     3.9 MB/sec    1.00      6.5±0.14ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.12ms     5.3 MB/sec    1.01      7.7±0.17ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1652.2±36.32µs    10.1 MB/sec    1.00  1639.8±33.83µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.4±4.38µs    16.4 MB/sec    1.00    180.6±4.47µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.06ms     7.5 MB/sec    1.02      3.5±0.07ms     7.3 MB/sec
parser/large/dataset.py                    1.02      6.3±0.14ms     6.5 MB/sec    1.00      6.1±0.09ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.02  1209.1±21.98µs    13.8 MB/sec    1.00  1190.2±27.58µs    14.0 MB/sec
parser/numpy/globals.py                    1.00    121.1±3.38µs    24.4 MB/sec    1.01    122.0±4.19µs    24.2 MB/sec
parser/pydantic/types.py                   1.03      2.6±0.06ms     9.7 MB/sec    1.00      2.6±0.06ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.0±0.07ms     2.5 MB/sec    1.00     15.9±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.00      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    484.5±6.58µs     6.1 MB/sec    1.00    484.3±5.70µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.05ms     3.8 MB/sec    1.00      6.8±0.04ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.09ms     5.0 MB/sec    1.00      8.1±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1720.4±14.62µs     9.7 MB/sec    1.00  1727.5±16.31µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.5±2.41µs    15.2 MB/sec    1.01    197.2±6.50µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.0 MB/sec    1.01      3.6±0.06ms     7.0 MB/sec
parser/large/dataset.py                    1.01      6.7±0.04ms     6.1 MB/sec    1.00      6.6±0.05ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.01  1252.1±21.51µs    13.3 MB/sec    1.00  1240.3±16.57µs    13.4 MB/sec
parser/numpy/globals.py                    1.01    126.5±2.55µs    23.3 MB/sec    1.00    125.7±2.26µs    23.5 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.02ms     9.2 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
