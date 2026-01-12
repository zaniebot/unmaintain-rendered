```yaml
number: 6350
title: "Avoid panic with positional-only arguments in `PYI019`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/posonly
created_at: 2023-08-04T18:19:34Z
updated_at: 2023-08-04T18:59:29Z
url: https://github.com/astral-sh/ruff/pull/6350
synced_at: 2026-01-12T02:52:04Z
```

# Avoid panic with positional-only arguments in `PYI019`

---

_Pull request opened by @charliermarsh on 2023-08-04 18:19_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Previously, failed on methods like:

```python
@classmethod
def bad_posonly_class_method(cls: type[_S], /) -> _S: ...  # PYI019
```

Since we check if there are any positional-only or non-positional arguments, but then do an unsafe access on `parameters.args`.

Closes https://github.com/astral-sh/ruff/issues/6349.

## Test Plan

`cargo test` (verified that `main` panics on the new fixtures)


---

_Label `bug` added by @charliermarsh on 2023-08-04 18:19_

---

_Merged by @charliermarsh on 2023-08-04 18:37_

---

_Closed by @charliermarsh on 2023-08-04 18:37_

---

_Branch deleted on 2023-08-04 18:37_

---

_Comment by @github-actions[bot] on 2023-08-04 18:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.02     10.2±0.51ms     4.0 MB/sec     1.00     10.0±0.45ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1979.4±160.53µs     8.4 MB/sec    1.01      2.0±0.12ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    222.2±9.82µs    13.3 MB/sec     1.05   232.7±13.18µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.17ms     6.5 MB/sec     1.10      4.3±0.17ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.02     13.9±0.37ms     2.9 MB/sec     1.00     13.6±0.49ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.06ms     4.9 MB/sec     1.01      3.4±0.09ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.05   506.7±28.62µs     5.8 MB/sec     1.00   482.9±26.98µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.4±0.38ms     4.0 MB/sec     1.00      6.2±0.27ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.4±0.27ms     6.4 MB/sec     1.01      6.4±0.21ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1389.8±121.31µs    12.0 MB/sec    1.03  1433.3±94.63µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.5±8.78µs    18.4 MB/sec     1.09    174.3±6.25µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.14ms     9.2 MB/sec     1.05      2.9±0.08ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.08ms     4.2 MB/sec    1.00      9.8±0.13ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1898.0±30.08µs     8.8 MB/sec    1.01  1908.5±36.09µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    213.5±4.63µs    13.8 MB/sec    1.01    215.2±8.25µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.06ms     6.2 MB/sec    1.01      4.1±0.09ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.01     13.4±0.18ms     3.0 MB/sec    1.00     13.3±0.21ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.8 MB/sec    1.00      3.5±0.05ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   431.9±27.10µs     6.8 MB/sec    1.00    428.5±5.92µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.09ms     4.2 MB/sec    1.00      6.1±0.11ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.02      6.8±0.10ms     6.0 MB/sec    1.00      6.7±0.10ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1401.3±24.77µs    11.9 MB/sec    1.00  1385.0±28.54µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.0±3.11µs    18.4 MB/sec    1.00    159.6±4.62µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.06ms     8.4 MB/sec    1.00      3.0±0.05ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
