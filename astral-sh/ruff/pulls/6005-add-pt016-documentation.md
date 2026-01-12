```yaml
number: 6005
title: "Add `PT016` documentation"
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: PT016-doc
created_at: 2023-07-23T02:30:14Z
updated_at: 2023-07-24T01:52:52Z
url: https://github.com/astral-sh/ruff/pull/6005
synced_at: 2026-01-12T15:55:20Z
```

# Add `PT016` documentation

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

- #2646
- Add `PT016` documentation

## Test Plan

<!-- How was it tested? -->

N/A


---

_Comment by @github-actions[bot] on 2023-07-23 03:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     11.0±0.44ms     3.7 MB/sec    1.00     10.9±0.44ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.11ms     7.7 MB/sec    1.02      2.2±0.17ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00   253.5±15.69µs    11.6 MB/sec    1.02   259.4±18.03µs    11.4 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.18ms     5.4 MB/sec    1.06      5.0±0.29ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.1±0.84ms     2.5 MB/sec    1.03     16.5±0.52ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.21ms     4.2 MB/sec    1.01      4.0±0.15ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   533.8±21.40µs     5.5 MB/sec    1.01   539.1±32.34µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.23ms     3.6 MB/sec    1.01      7.2±0.28ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.26ms     5.0 MB/sec    1.00      8.1±0.30ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1715.8±61.92µs     9.7 MB/sec    1.01  1726.5±77.42µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.02   205.1±12.84µs    14.4 MB/sec    1.00    201.7±8.82µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.19ms     7.0 MB/sec    1.01      3.7±0.15ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.15ms     3.7 MB/sec    1.00     11.1±0.18ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.01      2.2±0.06ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    242.0±5.70µs    12.2 MB/sec    1.03   248.5±28.34µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.05ms     5.4 MB/sec    1.01      4.8±0.08ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.6±0.12ms     2.6 MB/sec    1.00     15.5±0.15ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.1±0.04ms     4.1 MB/sec    1.00      4.0±0.04ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.02    487.3±9.17µs     6.1 MB/sec    1.00   475.5±11.99µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.11ms     3.6 MB/sec    1.00      7.0±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.07ms     5.0 MB/sec    1.00      8.0±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1711.4±19.51µs     9.7 MB/sec    1.00  1672.5±35.32µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    190.4±3.16µs    15.5 MB/sec    1.00    188.2±7.88µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_pytest_style/rules/fail.rs`:12 on 2023-07-23 07:16_

nit:
```suggestion
/// Checks for `pytest.fail` calls without a message.
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_pytest_style/rules/fail.rs`:48 on 2023-07-23 07:16_

nit:
```suggestion
/// - [API Reference: `pytest.fail`](https://docs.pytest.org/en/latest/reference/reference.html#pytest-fail)
```

---

_@dhruvmanila approved on 2023-07-23 07:16_

Thanks!

---

_Comment by @harupy on 2023-07-23 07:34_

@dhruvmanila Thanks for the suggestions!

---

_Merged by @charliermarsh on 2023-07-24 01:52_

---

_Closed by @charliermarsh on 2023-07-24 01:52_

---

_Label `documentation` added by @charliermarsh on 2023-07-24 01:52_

---
