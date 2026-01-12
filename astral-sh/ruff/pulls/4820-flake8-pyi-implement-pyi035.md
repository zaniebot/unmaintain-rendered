```yaml
number: 4820
title: "[`flake8-pyi`] Implement PYI035"
type: pull_request
state: merged
author: density
labels: []
assignees: []
merged: true
base: main
head: PYI035
created_at: 2023-06-02T23:47:10Z
updated_at: 2023-06-03T03:30:34Z
url: https://github.com/astral-sh/ruff/pull/4820
synced_at: 2026-01-12T15:55:16Z
```

# [`flake8-pyi`] Implement PYI035

---

_@density_

## Summary

Adds support for flake8-pyi rule Y035: `__all__ and __match_args__ in a stub file should always have values, as these special variables in a .pyi file have identical semantics to __all__ and __match_args__ in a .py file. E.g. write __all__ = ["foo", "bar"] instead of __all__: list[str].`

## Test Plan

Added snapshot tests

ref: #848 

---

_Marked ready for review by @density on 2023-06-02 23:54_

---

_Comment by @github-actions[bot] on 2023-06-03 00:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.2±0.06ms     2.7 MB/sec    1.00     15.1±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.01ms     4.5 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    375.4±2.42µs     7.9 MB/sec    1.00    373.9±1.15µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.01ms     4.0 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.02      7.6±0.01ms     5.3 MB/sec    1.00      7.5±0.04ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1609.0±3.55µs    10.3 MB/sec    1.00   1585.2±3.56µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    176.5±0.27µs    16.7 MB/sec    1.00    175.0±0.50µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.00ms     7.5 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.8±0.01ms     7.1 MB/sec    1.00      5.7±0.00ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1127.7±0.94µs    14.8 MB/sec    1.00   1123.9±1.04µs    14.8 MB/sec
parser/numpy/globals.py                    1.00    115.8±0.24µs    25.5 MB/sec    1.00    115.6±0.73µs    25.5 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.01ms    10.3 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.11ms     2.5 MB/sec    1.00     16.4±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.9±7.41µs     5.9 MB/sec    1.00    499.7±7.70µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.00      6.9±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.04      8.5±0.33ms     4.8 MB/sec    1.00      8.2±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1732.8±21.62µs     9.6 MB/sec    1.01  1756.2±15.59µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.7±3.14µs    14.6 MB/sec    1.01    204.0±3.92µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.01      3.7±0.04ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.4±0.05ms     6.4 MB/sec    1.00      6.4±0.05ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1196.7±9.11µs    13.9 MB/sec    1.00  1197.5±10.34µs    13.9 MB/sec
parser/numpy/globals.py                    1.00    123.3±1.56µs    23.9 MB/sec    1.00    123.7±3.54µs    23.9 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.03ms     9.4 MB/sec    1.00      2.7±0.03ms     9.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-03 03:13_

---

_Closed by @charliermarsh on 2023-06-03 03:13_

---
