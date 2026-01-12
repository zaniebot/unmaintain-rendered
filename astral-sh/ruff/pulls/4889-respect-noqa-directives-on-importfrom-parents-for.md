```yaml
number: 4889
title: "Respect noqa directives on `ImportFrom` parents for type-checking rules"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/typing
created_at: 2023-06-06T02:31:34Z
updated_at: 2023-06-06T02:59:16Z
url: https://github.com/astral-sh/ruff/pull/4889
synced_at: 2026-01-12T03:43:29Z
```

# Respect noqa directives on `ImportFrom` parents for type-checking rules

---

_Pull request opened by @charliermarsh on 2023-06-06 02:31_

## Summary

Similar to unused imports, we need to respect `noqa` directives like:

```py
from pandas import (  # noqa: TCH002
    DataFrame,
)
```

---

_Merged by @charliermarsh on 2023-06-06 02:37_

---

_Closed by @charliermarsh on 2023-06-06 02:37_

---

_Branch deleted on 2023-06-06 02:37_

---

_Comment by @github-actions[bot] on 2023-06-06 02:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.2±0.02ms     6.5 MB/sec    1.00      6.3±0.02ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1274.6±2.61µs    13.1 MB/sec    1.00   1269.8±1.77µs    13.1 MB/sec
formatter/numpy/globals.py                 1.00    145.6±0.46µs    20.3 MB/sec    1.00    145.0±2.66µs    20.3 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.3 MB/sec    1.00      2.7±0.01ms     9.3 MB/sec
linter/all-rules/large/dataset.py          1.00     14.8±0.12ms     2.8 MB/sec    1.00     14.7±0.13ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.5±1.18µs     8.2 MB/sec    1.00    360.3±1.97µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.05ms     4.1 MB/sec    1.00      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.02ms     5.5 MB/sec    1.02      7.5±0.05ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1544.1±4.93µs    10.8 MB/sec    1.00   1541.7±2.08µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.9±0.21µs    17.9 MB/sec    1.00    165.6±0.15µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.01      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.2±0.10ms     5.6 MB/sec    1.08      7.8±0.13ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1458.0±27.67µs    11.4 MB/sec    1.06  1540.5±33.63µs    10.8 MB/sec
formatter/numpy/globals.py                 1.00    160.3±3.26µs    18.4 MB/sec    1.04    167.4±4.85µs    17.6 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.08ms     8.1 MB/sec    1.07      3.4±0.08ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.00     18.1±0.28ms     2.3 MB/sec    1.01     18.3±0.26ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.11ms     3.8 MB/sec    1.03      4.6±0.12ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.5±9.23µs     5.9 MB/sec    1.04   517.3±11.40µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.12ms     3.5 MB/sec    1.07      7.9±0.18ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.15ms     4.7 MB/sec    1.06      9.1±0.15ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1804.2±77.05µs     9.2 MB/sec    1.05  1903.2±40.30µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.6±8.73µs    14.7 MB/sec    1.02    203.9±3.91µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.11ms     6.7 MB/sec    1.08      4.1±0.11ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
