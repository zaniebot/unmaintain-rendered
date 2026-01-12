```yaml
number: 5986
title: "Fix `F507` false positive"
type: pull_request
state: merged
author: harupy
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-F507-false-positive
created_at: 2023-07-22T16:14:18Z
updated_at: 2023-07-23T00:12:04Z
url: https://github.com/astral-sh/ruff/pull/5986
synced_at: 2026-01-12T03:30:22Z
```

# Fix `F507` false positive

---

_Pull request opened by @harupy on 2023-07-22 16:14_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

F507 should not be raised when the right-hand side value is a non-tuple object.

```python
'%s' % (1, 2, 3)  # throws
'%s' % [1, 2, 3]  # doesn't throw
'%s' % {1, 2, 3}  # doesn't throw
```

## Test Plan

<!-- How was it tested? -->

New test cases

---

_Comment by @harupy on 2023-07-22 16:15_

pyflakes' source code for F507:

https://github.com/PyCQA/pyflakes/blob/afe2c4dbe57b0cf3c86ec96a7e622163b2171b4a/pyflakes/checker.py#L1657

```python
        if (
                isinstance(node.right, (ast.List, ast.Tuple)) and
                # does not have any *splats (py35+ feature)
                not any(
                    isinstance(elt, ast.Starred)
                    for elt in node.right.elts
                )
        ):
```

Not sure if this is a mistake or intended...

---

_Comment by @github-actions[bot] on 2023-07-22 16:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.3±0.01ms     4.4 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1849.5±2.46µs     9.0 MB/sec    1.00   1855.5±2.23µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    202.8±0.32µs    14.5 MB/sec    1.01    203.9±0.36µs    14.5 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.01      4.0±0.02ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.8±0.06ms     3.2 MB/sec    1.07     13.8±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.1 MB/sec    1.05      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.1±0.44µs     6.8 MB/sec    1.03    445.6±0.83µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.06      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.2 MB/sec    1.14      7.5±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1409.2±1.57µs    11.8 MB/sec    1.11   1560.2±6.27µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.0±0.31µs    18.9 MB/sec    1.07    166.7±0.19µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.12      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.15ms     3.7 MB/sec    1.00     11.0±0.15ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.04ms     7.7 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    242.7±7.12µs    12.2 MB/sec    1.01    245.8±4.95µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.4 MB/sec    1.03      4.8±0.14ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.5±0.16ms     2.6 MB/sec    1.00     15.3±0.16ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.04ms     4.1 MB/sec    1.00      4.0±0.06ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.04    490.2±9.63µs     6.0 MB/sec    1.00    470.0±7.44µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.9±0.07ms     3.7 MB/sec    1.00      6.8±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.07ms     5.0 MB/sec    1.00      8.0±0.10ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1672.1±24.67µs    10.0 MB/sec    1.00  1667.6±15.97µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.03    190.9±2.90µs    15.5 MB/sec    1.00    184.9±3.25µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.04ms     7.1 MB/sec    1.00      3.5±0.04ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Fix `F507` fasel positive" to "Fix `F507` false positive" by @harupy on 2023-07-22 17:39_

---

_Label `bug` added by @charliermarsh on 2023-07-22 18:34_

---

_Merged by @charliermarsh on 2023-07-22 18:42_

---

_Closed by @charliermarsh on 2023-07-22 18:42_

---

_Branch deleted on 2023-07-23 00:12_

---
