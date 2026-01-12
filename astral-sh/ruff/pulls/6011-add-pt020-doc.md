```yaml
number: 6011
title: "Add `PT020` doc"
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: PT020-doc
created_at: 2023-07-23T08:24:48Z
updated_at: 2023-07-24T01:37:07Z
url: https://github.com/astral-sh/ruff/pull/6011
synced_at: 2026-01-12T03:30:22Z
```

# Add `PT020` doc

---

_Pull request opened by @harupy on 2023-07-23 08:24_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

#2646 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-07-23 08:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08     10.2±0.42ms     4.0 MB/sec    1.00      9.5±0.40ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1907.4±80.64µs     8.7 MB/sec    1.05  1997.1±106.88µs     8.3 MB/sec
formatter/numpy/globals.py                 1.15   242.4±16.38µs    12.2 MB/sec    1.00   211.0±14.74µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.24ms     5.9 MB/sec    1.02      4.4±0.20ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.05     14.3±1.24ms     2.8 MB/sec    1.00     13.6±0.77ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.6±0.16ms     4.6 MB/sec    1.00      3.5±0.19ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.04   454.5±20.16µs     6.5 MB/sec    1.00   438.1±20.99µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.36ms     4.2 MB/sec    1.00      6.1±0.25ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.23ms     6.1 MB/sec    1.04      6.9±0.34ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1432.5±82.91µs    11.6 MB/sec    1.01  1441.3±59.56µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.03   171.0±12.74µs    17.3 MB/sec    1.00   166.6±13.37µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.10ms     8.5 MB/sec    1.01      3.0±0.12ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.3±0.26ms     3.6 MB/sec    1.08     12.2±0.27ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.06ms     7.6 MB/sec    1.07      2.3±0.05ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00   248.7±10.57µs    11.9 MB/sec    1.03    255.1±5.97µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.20ms     5.3 MB/sec    1.07      5.2±0.12ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.22ms     2.6 MB/sec    1.01     16.0±0.32ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.08ms     4.1 MB/sec    1.01      4.1±0.11ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   488.9±14.92µs     6.0 MB/sec    1.01   493.0±11.81µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.11ms     3.6 MB/sec    1.00      7.1±0.17ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.13ms     5.0 MB/sec    1.00      8.1±0.13ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1694.4±31.66µs     9.8 MB/sec    1.00  1686.6±23.53µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    193.0±5.74µs    15.3 MB/sec    1.00    190.5±4.14µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.08ms     6.9 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @sbrugman on `crates/ruff/src/rules/flake8_pytest_style/rules/fixture.rs`:148 on 2023-07-23 15:38_

Missing closing bracket

---

_@sbrugman reviewed on 2023-07-23 15:38_

---

_@harupy reviewed on 2023-07-23 15:40_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/fixture.rs`:148 on 2023-07-23 15:40_

```suggestion
/// - [`yield_fixture` functions](https://docs.pytest.org/en/latest/yieldfixture.html)
```

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/fixture.rs`:148 on 2023-07-23 15:40_

Thanks for the catch!

---

_@harupy reviewed on 2023-07-23 15:40_

---

_Merged by @charliermarsh on 2023-07-24 01:37_

---

_Closed by @charliermarsh on 2023-07-24 01:37_

---

_Label `documentation` added by @charliermarsh on 2023-07-24 01:37_

---
