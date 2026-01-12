```yaml
number: 6182
title: Avoid falsely marking non-submodules as submodule aliases
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/submodule
created_at: 2023-07-30T22:08:46Z
updated_at: 2023-07-30T22:32:53Z
url: https://github.com/astral-sh/ruff/pull/6182
synced_at: 2026-01-12T15:55:20Z
```

# Avoid falsely marking non-submodules as submodule aliases

---

_@charliermarsh_

## Summary

We have some code to ensure that if an aliased import is used, any submodules should be marked as used too. This comment says it best:

```rust
// If the name of a submodule import is the same as an alias of another import, and the
// alias is used, then the submodule import should be marked as used too.
//
// For example, mark `pyarrow.csv` as used in:
//
// ```python
// import pyarrow as pa
// import pyarrow.csv
// print(pa.csv.read_csv("test.csv"))
// ```
```

However, it looks like when we go to look up `pyarrow` (of `import pyarrow as pa`), we aren't checking to ensure the resolved binding is _actually_ an import. This was causing us to attribute `print(rm.ANY)` to `def requests_mock` here:

```python
import requests_mock as rm

def requests_mock(requests_mock: rm.Mocker):
    print(rm.ANY)
```

Closes https://github.com/astral-sh/ruff/issues/6180.


---

_Label `bug` added by @charliermarsh on 2023-07-30 22:08_

---

_Merged by @charliermarsh on 2023-07-30 22:16_

---

_Closed by @charliermarsh on 2023-07-30 22:16_

---

_Branch deleted on 2023-07-30 22:16_

---

_Comment by @github-actions[bot] on 2023-07-30 22:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.9±0.02ms     4.5 MB/sec    1.01      9.0±0.02ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1731.4±9.68µs     9.6 MB/sec    1.00  1729.2±25.69µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    181.9±0.40µs    16.2 MB/sec    1.00    182.3±0.88µs    16.2 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.02ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.01     11.7±0.09ms     3.5 MB/sec    1.00     11.6±0.02ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.01ms     5.6 MB/sec    1.00      3.0±0.01ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    321.0±1.74µs     9.2 MB/sec    1.01    322.8±0.71µs     9.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.01ms     4.8 MB/sec    1.00      5.3±0.01ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.01ms     6.3 MB/sec    1.00      6.5±0.01ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1285.9±2.73µs    12.9 MB/sec    1.00   1285.6±3.47µs    13.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    127.2±0.25µs    23.2 MB/sec    1.00    127.2±0.28µs    23.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.8±0.01ms     9.0 MB/sec    1.00      2.8±0.01ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.5±0.07ms     5.4 MB/sec    1.00      7.5±0.06ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1424.6±46.16µs    11.7 MB/sec    1.00  1420.6±26.71µs    11.7 MB/sec
formatter/numpy/globals.py                 1.00    145.5±1.29µs    20.3 MB/sec    1.01    147.2±2.43µs    20.0 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.05ms     8.0 MB/sec    1.00      3.2±0.05ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00      9.9±0.06ms     4.1 MB/sec    1.00     10.0±0.07ms     4.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.02ms     6.2 MB/sec    1.01      2.7±0.03ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    269.9±4.13µs    10.9 MB/sec    1.00    269.7±3.96µs    10.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.5±0.03ms     5.6 MB/sec    1.02      4.6±0.07ms     5.5 MB/sec
linter/default-rules/large/dataset.py      1.00      5.6±0.03ms     7.3 MB/sec    1.00      5.6±0.06ms     7.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1104.3±8.50µs    15.1 MB/sec    1.00  1105.1±10.84µs    15.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    109.7±2.21µs    26.9 MB/sec    1.00    109.3±1.58µs    27.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.02ms    10.4 MB/sec    1.00      2.4±0.03ms    10.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
