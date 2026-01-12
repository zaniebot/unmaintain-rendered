```yaml
number: 6486
title: "[`pylint`] Implement `bad-dunder-name` (`W3201`)"
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
assignees: []
merged: true
base: main
head: PLW3201
created_at: 2023-08-10T21:20:52Z
updated_at: 2023-12-01T23:46:41Z
url: https://github.com/astral-sh/ruff/pull/6486
synced_at: 2026-01-10T23:40:55Z
```

# [`pylint`] Implement `bad-dunder-name` (`W3201`)

---

_Pull request opened by @LaBatata101 on 2023-08-10 21:20_

## Summary

Checks for any misspelled dunder name method and for any method defined with `__...__` that's not one of the pre-defined methods.

The pre-defined methods encompass all of Python's standard dunder methods.

ref: #970

## Test Plan
Snapshots and manual runs of pylint.


---

_Comment by @github-actions[bot] on 2023-08-10 21:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.11ms     4.3 MB/sec    1.00      9.6±0.09ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1905.1±25.35µs     8.7 MB/sec    1.01  1922.5±22.59µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    217.0±4.05µs    13.6 MB/sec    1.01    218.2±4.93µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.04ms     6.3 MB/sec    1.02      4.1±0.04ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     11.6±0.24ms     3.5 MB/sec    1.03     11.9±0.07ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.2±0.04ms     5.2 MB/sec    1.00      3.2±0.09ms     5.3 MB/sec
linter/all-rules/numpy/globals.py          1.02    457.0±5.61µs     6.5 MB/sec    1.00   446.2±10.72µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.3±0.04ms     4.1 MB/sec    1.00      6.0±0.12ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.2±0.07ms     6.6 MB/sec    1.00      6.2±0.09ms     6.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1352.3±11.71µs    12.3 MB/sec    1.00  1346.0±17.39µs    12.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    154.3±2.67µs    19.1 MB/sec    1.00    154.6±2.64µs    19.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.1±0.24ms     3.4 MB/sec    1.05     12.7±0.49ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.10ms     7.0 MB/sec    1.01      2.4±0.08ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00   259.5±11.06µs    11.4 MB/sec    1.03   267.7±15.37µs    11.0 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.20ms     4.9 MB/sec    1.00      5.2±0.15ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.01     15.7±0.45ms     2.6 MB/sec    1.00     15.6±0.41ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.13ms     3.9 MB/sec    1.00      4.2±0.20ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.04   542.0±25.10µs     5.4 MB/sec    1.00   523.0±16.24µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.04      8.2±0.30ms     3.1 MB/sec    1.00      8.0±0.21ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.01      8.6±0.16ms     4.7 MB/sec    1.00      8.5±0.26ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1764.1±44.89µs     9.4 MB/sec    1.02  1797.9±86.57µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.9±8.50µs    14.5 MB/sec    1.00   203.4±14.13µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.12ms     6.6 MB/sec    1.00      3.8±0.10ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @LaBatata101 on 2023-08-10 21:51_

`__anext__`  shouldn't be reported in [disnake/iterators.py](https://github.com/DisnakeDev/disnake/blob/634f25f3b92f0507b0b7a088b55ba891dd2fd758/disnake/iterators.py#L115)

---

_Label `rule` added by @charliermarsh on 2023-08-11 01:07_

---

_Merged by @charliermarsh on 2023-08-11 01:31_

---

_Closed by @charliermarsh on 2023-08-11 01:31_

---

_Branch deleted on 2023-12-01 23:46_

---
