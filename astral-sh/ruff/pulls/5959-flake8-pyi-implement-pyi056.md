```yaml
number: 5959
title: "[`flake8-pyi`] Implement PYI056"
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI056
created_at: 2023-07-21T22:42:27Z
updated_at: 2023-07-27T21:04:00Z
url: https://github.com/astral-sh/ruff/pull/5959
synced_at: 2026-01-12T03:23:56Z
```

# [`flake8-pyi`] Implement PYI056

---

_Pull request opened by @LaBatata101 on 2023-07-21 22:42_

## Summary

Checks that `append`, `extend` and `remove` methods are not called on `__all__`.  See [original implementation](https://github.com/PyCQA/flake8-pyi/blob/2a86db8271dc500247a8a69419536240334731cf/pyi.py#L1133-L1138).

```
$ flake8 --select Y026 crates/ruff/resources/test/fixtures/flake8_pyi/PYI056.pyi

crates/ruff/resources/test/fixtures/flake8_pyi/PYI056.pyi:3:1: Y056 Calling ".append()" on "__all__" may not be supported by all type checkers (use += instead)
crates/ruff/resources/test/fixtures/flake8_pyi/PYI056.pyi:4:1: Y056 Calling ".extend()" on "__all__" may not be supported by all type checkers (use += instead)
crates/ruff/resources/test/fixtures/flake8_pyi/PYI056.pyi:5:1: Y056 Calling ".remove()" on "__all__" may not be supported by all type checkers (use += instead)
```

```
$ ./target/debug/ruff --select PYI026 crates/ruff/resources/test/fixtures/flake8_pyi/PYI056.pyi --no-cache

crates/ruff/resources/test/fixtures/flake8_pyi/PYI056.pyi:3:1: PYI056 Calling ".append()" on "__all__" may not be supported by all type checkers (use += instead)
crates/ruff/resources/test/fixtures/flake8_pyi/PYI056.pyi:4:1: PYI056 Calling ".extend()" on "__all__" may not be supported by all type checkers (use += instead)
crates/ruff/resources/test/fixtures/flake8_pyi/PYI056.pyi:5:1: PYI056 Calling ".remove()" on "__all__" may not be supported by all type checkers (use += instead)
Found 3 errors.
```

ref #848

## Test Plan

Snapshots and manual runs of flake8.


---

_Comment by @github-actions[bot] on 2023-07-21 23:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.3±0.05ms     4.4 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1849.2±1.53µs     9.0 MB/sec    1.00   1847.9±1.75µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    203.4±2.40µs    14.5 MB/sec    1.00    203.5±1.86µs    14.5 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.03ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.11ms     3.1 MB/sec    1.00     13.0±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.0 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.1±2.54µs     6.9 MB/sec    1.01    433.4±0.64µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.4 MB/sec    1.01      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.03ms     6.2 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1408.0±1.57µs    11.8 MB/sec    1.00   1411.0±3.10µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    154.9±0.24µs    19.1 MB/sec    1.00    155.1±0.35µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.10ms     3.8 MB/sec    1.00     10.8±0.09ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.04ms     7.8 MB/sec    1.00      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    239.5±4.66µs    12.3 MB/sec    1.01    240.8±4.73µs    12.3 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.06ms     5.5 MB/sec    1.01      4.7±0.05ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.14ms     2.7 MB/sec    1.01     15.4±0.17ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.01      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    484.7±6.48µs     6.1 MB/sec    1.01   490.1±16.62µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.01      7.0±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.07ms     5.1 MB/sec    1.01      8.0±0.08ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1660.6±21.03µs    10.0 MB/sec    1.00  1665.4±19.36µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.6±2.49µs    15.6 MB/sec    1.00    190.3±3.60µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.06ms     7.2 MB/sec    1.00      3.5±0.03ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-07-22 04:08_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-07-22 04:08_

---

_Merged by @charliermarsh on 2023-07-22 04:25_

---

_Closed by @charliermarsh on 2023-07-22 04:25_

---

_Branch deleted on 2023-07-27 21:04_

---
