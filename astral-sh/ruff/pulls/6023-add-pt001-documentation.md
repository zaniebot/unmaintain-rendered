```yaml
number: 6023
title: "Add `PT001` documentation"
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: PT001-doc
created_at: 2023-07-24T05:18:11Z
updated_at: 2023-07-26T18:28:12Z
url: https://github.com/astral-sh/ruff/pull/6023
synced_at: 2026-01-12T15:55:20Z
```

# Add `PT001` documentation

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

#2646 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-07-24 05:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.5±0.15ms     4.8 MB/sec    1.09      9.3±0.05ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1696.4±33.60µs     9.8 MB/sec    1.10   1858.5±2.05µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    189.7±5.37µs    15.6 MB/sec    1.11    209.9±1.31µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.02ms     7.1 MB/sec    1.14      4.1±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.1±0.22ms     3.4 MB/sec    1.08     13.0±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.03ms     5.5 MB/sec    1.09      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    397.3±3.28µs     7.4 MB/sec    1.09    433.3±0.64µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.12ms     4.6 MB/sec    1.09      6.0±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      5.9±0.10ms     6.9 MB/sec    1.11      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1239.3±21.63µs    13.4 MB/sec    1.14   1412.3±1.52µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    140.4±5.83µs    21.0 MB/sec    1.12    157.2±0.36µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.7±0.06ms     9.6 MB/sec    1.13      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.5±0.26ms     3.9 MB/sec    1.05     11.0±0.15ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1992.1±35.66µs     8.4 MB/sec    1.10      2.2±0.04ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    215.9±4.72µs    13.7 MB/sec    1.16   251.1±15.20µs    11.8 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.06ms     5.8 MB/sec    1.10      4.8±0.07ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     14.6±0.22ms     2.8 MB/sec    1.09     16.0±0.24ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.05ms     4.4 MB/sec    1.09      4.1±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   460.3±13.71µs     6.4 MB/sec    1.06    488.9±5.89µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.08ms     3.9 MB/sec    1.11      7.2±0.11ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.10ms     5.3 MB/sec    1.07      8.2±0.10ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1521.1±24.34µs    10.9 MB/sec    1.12  1710.3±20.85µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.5±3.50µs    17.7 MB/sec    1.17    195.5±4.00µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.05ms     7.7 MB/sec    1.11      3.7±0.04ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@tjkuson reviewed on 2023-07-24 09:52_

---

_Review comment by @tjkuson on `crates/ruff/src/rules/flake8_pytest_style/rules/fixture.rs`:53 on 2023-07-24 09:52_

```suggestion
/// - `flake8-pytest-style.fixture-parentheses`
///
/// ## References
/// - [API Reference: Fixtures](https://docs.pytest.org/en/latest/reference/reference.html#fixtures-api)
```

---

_Review comment by @tjkuson on `crates/ruff/src/rules/flake8_pytest_style/rules/fixture.rs`:53 on 2023-07-24 09:53_

It might be helpful to add a link to the docs (they also use the decorator without parentheses).

---

_@tjkuson reviewed on 2023-07-24 09:54_

---

_@harupy reviewed on 2023-07-24 09:57_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/fixture.rs`:53 on 2023-07-24 09:57_

Sounds good. Let's add it.

---

_@harupy reviewed on 2023-07-24 09:58_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/fixture.rs`:52 on 2023-07-24 09:58_

```suggestion
///
/// ## Options
```

---

_Label `documentation` added by @charliermarsh on 2023-07-26 17:54_

---

_Merged by @charliermarsh on 2023-07-26 18:05_

---

_Closed by @charliermarsh on 2023-07-26 18:05_

---
