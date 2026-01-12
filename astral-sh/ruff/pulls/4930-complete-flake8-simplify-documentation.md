```yaml
number: 4930
title: "Complete `flake8-simplify` documentation"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: simplify-docs
created_at: 2023-06-07T15:02:45Z
updated_at: 2023-07-10T09:55:28Z
url: https://github.com/astral-sh/ruff/pull/4930
synced_at: 2026-01-12T15:55:17Z
```

# Complete `flake8-simplify` documentation

---

_@tjkuson_

## Summary

Completes the documentation for the `flake8-simplify` rules.

Related to #2646.

## Test Plan

`cargo fmt && python scripts/check_docs_formatted.py`


---

_Comment by @github-actions[bot] on 2023-06-07 15:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.22      7.8±0.33ms     5.2 MB/sec    1.00      6.4±0.30ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1361.1±57.89µs    12.2 MB/sec    1.00  1362.0±67.69µs    12.2 MB/sec
formatter/numpy/globals.py                 1.03    166.5±9.72µs    17.7 MB/sec    1.00    161.9±9.74µs    18.2 MB/sec
formatter/pydantic/types.py                1.02      3.3±0.14ms     7.7 MB/sec    1.00      3.2±0.16ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.9±0.84ms     2.4 MB/sec    1.02     17.2±0.90ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.49ms     4.0 MB/sec    1.00      4.1±0.23ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   507.2±25.47µs     5.8 MB/sec    1.00   496.6±24.66µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.39ms     3.6 MB/sec    1.00      7.1±0.46ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.34ms     5.2 MB/sec    1.00      7.9±0.43ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1688.9±71.65µs     9.9 MB/sec    1.00  1665.2±96.47µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   201.9±16.73µs    14.6 MB/sec    1.04   210.8±11.28µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.18ms     7.2 MB/sec    1.04      3.7±0.16ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.08      9.0±0.45ms     4.5 MB/sec     1.00      8.3±0.55ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.07  1665.8±84.04µs    10.0 MB/sec     1.00  1555.3±109.89µs    10.7 MB/sec
formatter/numpy/globals.py                 1.04   196.0±17.67µs    15.1 MB/sec     1.00   188.1±20.39µs    15.7 MB/sec
formatter/pydantic/types.py                1.03      3.8±0.20ms     6.8 MB/sec     1.00      3.7±0.32ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.00     19.5±1.00ms     2.1 MB/sec     1.02     19.9±1.08ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.39ms     3.5 MB/sec     1.06      5.1±0.24ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.03   591.0±33.96µs     5.0 MB/sec     1.00   574.0±24.18µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.1±0.56ms     3.1 MB/sec     1.04      8.4±0.42ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.60ms     4.1 MB/sec     1.01     10.0±0.56ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1937.4±117.82µs     8.6 MB/sec    1.11      2.1±0.10ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.03   253.4±28.50µs    11.6 MB/sec     1.00   245.1±19.16µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.6±0.27ms     5.5 MB/sec     1.00      4.5±0.25ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-08 19:58_

---

_Label `documentation` added by @charliermarsh on 2023-06-09 01:53_

---

_Comment by @charliermarsh on 2023-06-09 01:53_

Thank you!

---

_Merged by @charliermarsh on 2023-06-09 02:02_

---

_Closed by @charliermarsh on 2023-06-09 02:02_

---

_Branch deleted on 2023-07-10 09:55_

---
