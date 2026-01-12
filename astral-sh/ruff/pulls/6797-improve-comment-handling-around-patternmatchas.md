```yaml
number: 6797
title: "Improve comment handling around `PatternMatchAs`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/pattern-match-as
created_at: 2023-08-22T23:54:39Z
updated_at: 2023-08-23T05:08:32Z
url: https://github.com/astral-sh/ruff/pull/6797
synced_at: 2026-01-12T02:45:38Z
```

# Improve comment handling around `PatternMatchAs`

---

_Pull request opened by @charliermarsh on 2023-08-22 23:54_

## Summary

Follows up on https://github.com/astral-sh/ruff/pull/6652#discussion_r1300871033 with some modifications to the `PatternMatchAs` comment handling. Specifically, any comments between the `as` and the end are now formatted as dangling, and we now insert some newlines in the appropriate places.

## Test Plan

`cargo test`


---

_Renamed from "Improve comment handling around PatternMatchAs" to "Improve comment handling around `PatternMatchAs`" by @charliermarsh on 2023-08-22 23:54_

---

_Label `formatter` added by @charliermarsh on 2023-08-22 23:54_

---

_Converted to draft by @charliermarsh on 2023-08-23 01:17_

---

_Comment by @github-actions[bot] on 2023-08-23 01:21_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.01ms    12.3 MB/sec    1.01      3.3±0.02ms    12.2 MB/sec
formatter/numpy/ctypeslib.py               1.00    673.7±6.86µs    24.7 MB/sec    1.01   681.7±12.75µs    24.4 MB/sec
formatter/numpy/globals.py                 1.00     71.5±0.40µs    41.3 MB/sec    1.01     72.0±1.42µs    41.0 MB/sec
formatter/pydantic/types.py                1.00  1349.3±16.32µs    18.9 MB/sec    1.00  1352.8±35.63µs    18.9 MB/sec
linter/all-rules/large/dataset.py          1.00     10.5±0.04ms     3.9 MB/sec    1.10     11.5±0.17ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.8 MB/sec    1.04      3.0±0.01ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    317.2±0.82µs     9.3 MB/sec    1.02    322.4±1.43µs     9.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.03ms     4.7 MB/sec    1.04      5.7±0.02ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.3 MB/sec    1.14      6.3±0.02ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1178.4±2.37µs    14.1 MB/sec    1.10   1294.2±4.99µs    12.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    123.3±0.31µs    23.9 MB/sec    1.06    130.4±0.32µs    22.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.2 MB/sec    1.11      2.8±0.01ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.6±0.02ms    11.2 MB/sec    1.00      3.6±0.03ms    11.1 MB/sec
formatter/numpy/ctypeslib.py               1.00    722.3±8.77µs    23.1 MB/sec    1.01   729.3±17.82µs    22.8 MB/sec
formatter/numpy/globals.py                 1.00     74.5±0.85µs    39.6 MB/sec    1.01     75.0±1.24µs    39.3 MB/sec
formatter/pydantic/types.py                1.00  1488.9±16.61µs    17.1 MB/sec    1.01  1502.8±16.77µs    17.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.4±0.07ms     3.3 MB/sec    1.01     12.5±0.07ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.9±5.40µs     8.2 MB/sec    1.02    367.6±4.20µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.03ms     3.9 MB/sec    1.00      6.5±0.04ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.03ms     5.8 MB/sec    1.01      7.0±0.03ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1469.2±10.08µs    11.3 MB/sec    1.00  1463.0±12.37µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    147.1±1.04µs    20.1 MB/sec    1.03    150.8±2.75µs    19.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.1 MB/sec    1.00      3.2±0.02ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @charliermarsh on 2023-08-23 01:36_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-23 01:36_

---

_Review requested from @konstin by @charliermarsh on 2023-08-23 01:36_

---

_@dhruvmanila approved on 2023-08-23 04:06_

---

_@dhruvmanila reviewed on 2023-08-23 04:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:180 on 2023-08-23 04:09_

Could we add a test case with just end of line comments?
```python
match subject:
    case (
        pattern # 1
        as # 2
        name # 3
    ):
        pass
```

Black collapses them but I don't think we should.

---

_Merged by @charliermarsh on 2023-08-23 04:48_

---

_Closed by @charliermarsh on 2023-08-23 04:48_

---

_Branch deleted on 2023-08-23 04:48_

---
