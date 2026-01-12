```yaml
number: 4771
title: Remove erroneous method calls in flake8-unused-arguments docs
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/arg-docs
created_at: 2023-06-01T02:14:52Z
updated_at: 2023-06-01T06:59:52Z
url: https://github.com/astral-sh/ruff/pull/4771
synced_at: 2026-01-12T15:55:16Z
```

# Remove erroneous method calls in flake8-unused-arguments docs

---

_@charliermarsh_

Closes #4765.

---

_Label `documentation` added by @charliermarsh on 2023-06-01 02:14_

---

_Merged by @charliermarsh on 2023-06-01 02:23_

---

_Closed by @charliermarsh on 2023-06-01 02:23_

---

_Branch deleted on 2023-06-01 02:24_

---

_Comment by @github-actions[bot] on 2023-06-01 02:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.04ms     2.7 MB/sec    1.00     15.0±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    375.6±1.74µs     7.9 MB/sec    1.00    375.3±0.73µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.01ms     4.0 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.01ms     5.4 MB/sec    1.00      7.5±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1597.0±2.71µs    10.4 MB/sec    1.00   1604.0±4.53µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.8±0.41µs    16.6 MB/sec    1.00    178.6±1.22µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.7±0.00ms     7.1 MB/sec    1.00      5.7±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1121.8±2.15µs    14.8 MB/sec    1.00   1120.8±1.66µs    14.9 MB/sec
parser/numpy/globals.py                    1.00    115.0±0.28µs    25.7 MB/sec    1.01    116.1±0.80µs    25.4 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.4 MB/sec    1.00      2.4±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.1±0.59ms     2.4 MB/sec    1.00     17.0±0.23ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     3.9 MB/sec    1.00      4.2±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.6±5.61µs     5.9 MB/sec    1.01   503.9±16.65µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.08ms     3.6 MB/sec    1.01      7.1±0.14ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.07ms     4.9 MB/sec    1.00      8.3±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1780.3±16.86µs     9.4 MB/sec    1.00  1788.6±19.76µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.3±3.22µs    14.4 MB/sec    1.01    207.3±6.16µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.01      3.8±0.03ms     6.7 MB/sec
parser/large/dataset.py                    1.01      6.5±0.04ms     6.3 MB/sec    1.00      6.4±0.04ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1220.5±20.53µs    13.6 MB/sec    1.00  1216.3±18.31µs    13.7 MB/sec
parser/numpy/globals.py                    1.02    126.5±8.07µs    23.3 MB/sec    1.00    124.4±2.09µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.3 MB/sec    1.00      2.7±0.03ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @saippuakauppias on `crates/ruff/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:93 on 2023-06-01 06:59_

It seems to me that calling the class `Class` is not right, because there are `class` directive

---

_@saippuakauppias reviewed on 2023-06-01 06:59_

---
