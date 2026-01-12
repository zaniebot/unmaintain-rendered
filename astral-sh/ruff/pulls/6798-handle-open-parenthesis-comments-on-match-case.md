```yaml
number: 6798
title: Handle open-parenthesis comments on match case
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/parenthesized-case
created_at: 2023-08-23T00:21:26Z
updated_at: 2023-08-23T04:40:19Z
url: https://github.com/astral-sh/ruff/pull/6798
synced_at: 2026-01-12T15:55:22Z
```

# Handle open-parenthesis comments on match case

---

_@charliermarsh_

## Summary

Ensures that we retain the open-parenthesis comment in cases like:
```python
match pattern_comments:
    case (  # leading
        only_leading
    ):
        ...
```

Previously, this was treated as a leading comment on `only_leading`.

## Test Plan

`cargo test`


---

_@charliermarsh reviewed on 2023-08-23 00:21_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/match_case.rs`:37 on 2023-08-23 00:21_

This strikes me as unusual (shouldn't the pattern format its own leading comments?) and the tests seem to pass without it. Hmm...

---

_Label `formatter` added by @charliermarsh on 2023-08-23 00:21_

---

_Converted to draft by @charliermarsh on 2023-08-23 01:18_

---

_Comment by @github-actions[bot] on 2023-08-23 01:24_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.08ms    10.6 MB/sec    1.03      4.0±0.03ms    10.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   808.9±18.86µs    20.6 MB/sec    1.00   810.0±35.97µs    20.6 MB/sec
formatter/numpy/globals.py                 1.05     88.3±1.96µs    33.4 MB/sec    1.00     84.1±7.44µs    35.1 MB/sec
formatter/pydantic/types.py                1.00  1557.0±39.90µs    16.4 MB/sec    1.01  1573.5±41.93µs    16.2 MB/sec
linter/all-rules/large/dataset.py          1.02     11.7±0.17ms     3.5 MB/sec    1.00     11.6±0.17ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.2±0.05ms     5.3 MB/sec    1.00      3.1±0.05ms     5.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    444.4±6.85µs     6.6 MB/sec    1.00    445.7±8.54µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.10ms     4.2 MB/sec    1.00      6.1±0.12ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.2±0.10ms     6.6 MB/sec    1.00      6.2±0.09ms     6.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1372.5±20.56µs    12.1 MB/sec    1.00  1374.8±26.90µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.6±3.83µs    18.7 MB/sec    1.03    162.8±3.38µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.8±0.04ms     9.0 MB/sec    1.00      2.8±0.06ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.05ms    11.1 MB/sec    1.02      3.7±0.08ms    10.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   756.0±11.14µs    22.0 MB/sec    1.01   763.2±12.31µs    21.8 MB/sec
formatter/numpy/globals.py                 1.00     78.6±1.30µs    37.6 MB/sec    1.01     79.0±1.61µs    37.3 MB/sec
formatter/pydantic/types.py                1.00  1525.0±46.48µs    16.7 MB/sec    1.01  1540.4±29.51µs    16.6 MB/sec
linter/all-rules/large/dataset.py          1.01     12.6±0.20ms     3.2 MB/sec    1.00     12.5±0.13ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.07ms     4.8 MB/sec    1.00      3.4±0.04ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.8±5.76µs     6.8 MB/sec    1.01   438.3±10.03µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.6±0.14ms     3.8 MB/sec    1.00      6.5±0.07ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.06ms     5.8 MB/sec    1.00      7.0±0.07ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1475.8±12.63µs    11.3 MB/sec    1.00  1482.3±16.29µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.3±2.42µs    17.3 MB/sec    1.00    170.4±2.25µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.03ms     8.1 MB/sec    1.00      3.1±0.03ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @charliermarsh on 2023-08-23 01:36_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-23 01:36_

---

_Review requested from @konstin by @charliermarsh on 2023-08-23 01:36_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/match_case.rs`:37 on 2023-08-23 03:52_

I think the idea was to avoid handling comments for _each_ pattern and handle it at once in the `MatchCase` node. These would only include comments surrounding the pattern itself and not the ones which are in between the pattern (for example, comments between sequence element).

---

_@dhruvmanila reviewed on 2023-08-23 03:52_

---

_@dhruvmanila approved on 2023-08-23 04:15_

As of today this is a deviation from Black which collapses this into `case (pattern):  # comment`.

---

_Merged by @charliermarsh on 2023-08-23 04:40_

---

_Closed by @charliermarsh on 2023-08-23 04:40_

---

_Branch deleted on 2023-08-23 04:40_

---
