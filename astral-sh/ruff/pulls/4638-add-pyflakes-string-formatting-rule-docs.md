```yaml
number: 4638
title: Add Pyflakes string formatting rule docs
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: f5-rules
created_at: 2023-05-24T16:28:17Z
updated_at: 2023-05-29T15:34:44Z
url: https://github.com/astral-sh/ruff/pull/4638
synced_at: 2026-01-12T15:55:16Z
```

# Add Pyflakes string formatting rule docs

---

_@tjkuson_

## Summary

Add docs for the Pyflakes string formatting rules (F5).

Related to #2646.

## Test Plan

Ran `scripts/check_docs_formatted.py` on local machine.

---

_Comment by @github-actions[bot] on 2023-05-24 17:03_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     19.0±0.49ms     2.1 MB/sec    1.00     18.2±0.52ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.5±0.20ms     3.7 MB/sec    1.00      4.4±0.14ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   563.2±32.09µs     5.2 MB/sec    1.00   560.2±27.88µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.9±0.45ms     3.2 MB/sec    1.00      7.6±0.26ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.01      8.7±0.23ms     4.7 MB/sec    1.00      8.6±0.20ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1909.9±58.51µs     8.7 MB/sec    1.00  1903.3±73.20µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    227.3±8.26µs    13.0 MB/sec    1.02   232.4±10.57µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.13ms     6.4 MB/sec    1.01      4.0±0.17ms     6.3 MB/sec
parser/large/dataset.py                    1.00      6.6±0.28ms     6.1 MB/sec    1.00      6.7±0.17ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.03  1347.0±72.37µs    12.4 MB/sec    1.00  1312.5±39.18µs    12.7 MB/sec
parser/numpy/globals.py                    1.02    135.4±6.80µs    21.8 MB/sec    1.00    132.6±7.26µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.10ms     8.6 MB/sec    1.00      2.9±0.14ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     19.7±0.71ms     2.1 MB/sec    1.00     19.3±0.72ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.14ms     3.4 MB/sec    1.00      4.9±0.17ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.01   599.0±24.37µs     4.9 MB/sec    1.00   590.6±19.57µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.4±0.34ms     3.0 MB/sec    1.00      8.3±0.47ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.28ms     4.2 MB/sec    1.00      9.7±0.50ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.1±0.10ms     7.9 MB/sec    1.00      2.1±0.15ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.17   291.1±93.02µs    10.1 MB/sec    1.00   249.7±20.63µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.12      5.0±0.61ms     5.1 MB/sec    1.00      4.4±0.17ms     5.8 MB/sec
parser/large/dataset.py                    1.00      8.0±0.21ms     5.1 MB/sec    1.00      8.0±0.20ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.01  1556.3±85.05µs    10.7 MB/sec    1.00  1533.7±51.49µs    10.9 MB/sec
parser/numpy/globals.py                    1.01    157.0±6.81µs    18.8 MB/sec    1.00    155.3±5.70µs    19.0 MB/sec
parser/pydantic/types.py                   1.02      3.5±0.11ms     7.3 MB/sec    1.00      3.4±0.08ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Add F5 rule docs" to "Add Pylint string formatting rule docs" by @tjkuson on 2023-05-26 19:12_

---

_Merged by @charliermarsh on 2023-05-27 02:47_

---

_Closed by @charliermarsh on 2023-05-27 02:47_

---

_Renamed from "Add Pylint string formatting rule docs" to "Add Pyflakes string formatting rule docs" by @tjkuson on 2023-05-27 21:58_

---

_Branch deleted on 2023-05-29 15:34_

---
