```yaml
number: 4700
title: "[`flake8-pyi`] Implement `PYI045`"
type: pull_request
state: merged
author: density
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI045
created_at: 2023-05-29T04:39:06Z
updated_at: 2023-05-31T22:23:10Z
url: https://github.com/astral-sh/ruff/pull/4700
synced_at: 2026-01-12T15:55:16Z
```

# [`flake8-pyi`] Implement `PYI045`

---

_@density_

## Summary

Implement [flake8-pyi](https://github.com/PyCQA/flake8-pyi) rule Y045: `__iter__ methods should never return Iterable[T], as they should always return some kind of iterator.`

## Test Plan

Added snapshot tests

ref: #848 

---

_Renamed from "[flake8-pyi] Add PYI045" to "[`flake8-pyi`] Implement `PYI045`" by @density on 2023-05-29 04:39_

---

_Marked ready for review by @density on 2023-05-29 05:02_

---

_Comment by @github-actions[bot] on 2023-05-29 05:06_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.06ms     2.7 MB/sec    1.01     15.1±0.15ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    374.1±1.68µs     7.9 MB/sec    1.00    374.2±1.20µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.00      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1577.1±12.90µs    10.6 MB/sec    1.00   1572.4±4.34µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.2±1.34µs    17.0 MB/sec    1.00    172.7±2.55µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.01      5.8±0.00ms     7.1 MB/sec    1.00      5.7±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1129.9±3.71µs    14.7 MB/sec    1.00   1125.3±0.53µs    14.8 MB/sec
parser/numpy/globals.py                    1.00    115.0±0.28µs    25.7 MB/sec    1.00    114.6±2.10µs    25.7 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.3 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.6±0.29ms     2.6 MB/sec    1.00     15.4±0.38ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.07ms     4.3 MB/sec    1.00      3.8±0.10ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    450.5±8.88µs     6.5 MB/sec    1.01   452.9±12.15µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.12ms     4.0 MB/sec    1.00      6.4±0.10ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.11ms     5.4 MB/sec    1.01      7.6±0.13ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1603.6±28.09µs    10.4 MB/sec    1.00  1605.8±35.02µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.1±5.16µs    15.9 MB/sec    1.01    186.9±5.75µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.06ms     7.5 MB/sec    1.00      3.4±0.07ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.8±0.11ms     7.0 MB/sec    1.00      5.8±0.11ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1106.3±20.84µs    15.1 MB/sec    1.00  1107.9±24.60µs    15.0 MB/sec
parser/numpy/globals.py                    1.00    112.5±3.12µs    26.2 MB/sec    1.00    112.1±2.30µs    26.3 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.05ms    10.2 MB/sec    1.00      2.5±0.05ms    10.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-05-29 21:21_

---

_Comment by @charliermarsh on 2023-05-29 21:22_

We should do Y034 next if we can, since flake8-pyi ends up excluding some cases from Y045 to include them in Y034 instead.

---

_Merged by @charliermarsh on 2023-05-29 21:27_

---

_Closed by @charliermarsh on 2023-05-29 21:27_

---

_Comment by @qdegraaf on 2023-05-31 19:10_

> We should do Y034 next if we can, since flake8-pyi ends up excluding some cases from Y045 to include them in Y034 instead.

@charliermarsh got a draft up here: https://github.com/charliermarsh/ruff/pull/4764. Will try and find some time tomorrow to finish it up. 

---

_Branch deleted on 2023-05-31 22:23_

---
