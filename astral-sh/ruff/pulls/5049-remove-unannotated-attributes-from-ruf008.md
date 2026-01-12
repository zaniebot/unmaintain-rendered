```yaml
number: 5049
title: Remove unannotated attributes from RUF008
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/attributes
created_at: 2023-06-13T14:09:04Z
updated_at: 2023-06-13T14:40:11Z
url: https://github.com/astral-sh/ruff/pull/5049
synced_at: 2026-01-12T03:43:30Z
```

# Remove unannotated attributes from RUF008

---

_Pull request opened by @charliermarsh on 2023-06-13 14:09_

## Summary

In a dataclass:

```py
from dataclasses import dataclass

@dataclass
class X:
    class_var = {}
    x: int
```

`class_var` isn't actually a dataclass attribute, since it's unannotated. This PR removes such attributes from RUF008 (`mutable-dataclass-default`), but it does enforce them in RUF012 (`mutable-class-default`), since those should be annotated with `ClassVar` like any other mutable class attribute.

Closes #5043.


---

_Label `rule` added by @charliermarsh on 2023-06-13 14:09_

---

_Comment by @github-actions[bot] on 2023-06-13 14:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     5.9 MB/sec    1.02      7.0±0.04ms     5.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1402.9±3.62µs    11.9 MB/sec    1.01   1421.1±5.35µs    11.7 MB/sec
formatter/numpy/globals.py                 1.00    139.1±0.82µs    21.2 MB/sec    1.01    140.3±0.55µs    21.0 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.1 MB/sec    1.01      2.8±0.01ms     9.0 MB/sec
linter/all-rules/large/dataset.py          1.01     14.8±0.07ms     2.8 MB/sec    1.00     14.6±0.07ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    373.6±0.88µs     7.9 MB/sec    1.00    368.9±0.95µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.04ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.02ms     5.5 MB/sec    1.00      7.4±0.03ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1543.8±1.90µs    10.8 MB/sec    1.00   1546.8±2.10µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    166.6±0.13µs    17.7 MB/sec    1.00    165.5±0.30µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.21     11.4±0.30ms     3.6 MB/sec    1.00      9.4±0.23ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.19      2.3±0.11ms     7.2 MB/sec    1.00  1942.2±64.80µs     8.6 MB/sec
formatter/numpy/globals.py                 1.08   209.6±12.78µs    14.1 MB/sec    1.00   194.7±14.32µs    15.2 MB/sec
formatter/pydantic/types.py                1.17      4.6±0.18ms     5.6 MB/sec    1.00      3.9±0.13ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     20.4±0.36ms  2045.1 KB/sec    1.00     20.4±0.51ms  2047.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.15ms     3.2 MB/sec    1.02      5.3±0.24ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   616.6±18.76µs     4.8 MB/sec    1.00   611.6±20.39µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.9±0.23ms     2.9 MB/sec    1.00      8.8±0.27ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.02     10.4±0.23ms     3.9 MB/sec    1.00     10.3±0.22ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.07ms     7.5 MB/sec    1.00      2.2±0.35ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.02   250.0±12.50µs    11.8 MB/sec    1.00    246.2±9.84µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.7±0.18ms     5.4 MB/sec    1.00      4.6±0.10ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-13 14:21_

---

_Closed by @charliermarsh on 2023-06-13 14:21_

---

_Branch deleted on 2023-06-13 14:21_

---
