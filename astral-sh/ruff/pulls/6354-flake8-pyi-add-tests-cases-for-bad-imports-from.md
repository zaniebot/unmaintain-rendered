```yaml
number: 6354
title: "[`flake8-pyi`] Add tests cases for bad imports from PYI027 to PYI022 (UP035)"
type: pull_request
state: merged
author: LaBatata101
labels: []
assignees: []
merged: true
base: main
head: PYI022-update
created_at: 2023-08-04T22:25:53Z
updated_at: 2023-12-01T23:46:46Z
url: https://github.com/astral-sh/ruff/pull/6354
synced_at: 2026-01-10T23:40:55Z
```

# [`flake8-pyi`] Add tests cases for bad imports from PYI027 to PYI022 (UP035)

---

_Pull request opened by @LaBatata101 on 2023-08-04 22:25_

## Summary
As of version [23.1.0](https://github.com/PyCQA/flake8-pyi/blob/2a86db8271dc500247a8a69419536240334731cf/CHANGELOG.md?plain=1#L158-L160), `flake8-pyi` remove the rule `Y027`.

The errors that resulted in `PYI027` are now being emitted by `PYI022` (`UP035`).

ref: #848 

## Test Plan

Add new tests cases.


---

_Comment by @github-actions[bot] on 2023-08-04 22:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.03ms     4.9 MB/sec    1.01      8.4±0.03ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1609.5±8.66µs    10.3 MB/sec    1.00  1615.9±15.75µs    10.3 MB/sec
formatter/numpy/globals.py                 1.01    172.0±0.51µs    17.2 MB/sec    1.00    171.1±0.35µs    17.2 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.02ms     7.3 MB/sec    1.01      3.5±0.07ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.03     11.6±0.08ms     3.5 MB/sec    1.00     11.3±0.05ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.9±0.01ms     5.7 MB/sec    1.00      2.9±0.02ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    317.4±1.87µs     9.3 MB/sec    1.00    313.4±2.76µs     9.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.2±0.03ms     4.9 MB/sec    1.00      5.2±0.02ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.03      5.5±0.01ms     7.4 MB/sec    1.00      5.3±0.01ms     7.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1127.5±2.59µs    14.8 MB/sec    1.00   1102.9±6.01µs    15.1 MB/sec
linter/default-rules/numpy/globals.py      1.03    116.9±1.11µs    25.2 MB/sec    1.00    113.3±0.61µs    26.0 MB/sec
linter/default-rules/pydantic/types.py     1.04      2.5±0.01ms    10.2 MB/sec    1.00      2.4±0.01ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.11ms     4.1 MB/sec    1.02     10.0±0.13ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1920.1±27.65µs     8.7 MB/sec    1.01  1941.4±60.23µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    213.0±6.89µs    13.8 MB/sec    1.02    216.5±8.85µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.06ms     6.1 MB/sec    1.01      4.2±0.06ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.13ms     3.0 MB/sec    1.01     13.6±0.15ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.8 MB/sec    1.00      3.5±0.04ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   432.5±13.47µs     6.8 MB/sec    1.00    429.6±7.50µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.10ms     4.1 MB/sec    1.00      6.2±0.12ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.07ms     6.0 MB/sec    1.01      6.9±0.09ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1409.1±16.35µs    11.8 MB/sec    1.01  1416.4±17.90µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.6±3.36µs    18.5 MB/sec    1.00    159.8±3.53µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.4 MB/sec    1.01      3.1±0.04ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-04 23:00_

---

_Closed by @charliermarsh on 2023-08-04 23:00_

---

_Branch deleted on 2023-12-01 23:46_

---
