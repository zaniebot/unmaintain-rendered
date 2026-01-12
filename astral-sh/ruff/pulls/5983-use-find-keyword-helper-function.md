```yaml
number: 5983
title: "Use `find_keyword` helper function"
type: pull_request
state: merged
author: tjkuson
labels:
  - internal
assignees: []
merged: true
base: main
head: keyword-helper
created_at: 2023-07-22T14:32:18Z
updated_at: 2023-07-23T00:01:06Z
url: https://github.com/astral-sh/ruff/pull/5983
synced_at: 2026-01-12T15:55:20Z
```

# Use `find_keyword` helper function

---

_@tjkuson_

## Summary

Use `find_keyword` helper function instead of reimplementing it.

## Test Plan

`cargo test`


---

_Label `internal` added by @charliermarsh on 2023-07-22 14:34_

---

_Comment by @tjkuson on 2023-07-22 14:43_

I found another function that could be replaced and didn't realise you enabled auto-merge, sorry!

---

_Comment by @github-actions[bot] on 2023-07-22 14:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.1±0.32ms     3.4 MB/sec    1.00     12.1±0.50ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.4±0.18ms     6.8 MB/sec    1.00      2.4±0.07ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00   274.6±11.73µs    10.7 MB/sec    1.00   275.1±13.81µs    10.7 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.18ms     4.9 MB/sec    1.00      5.2±0.20ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     17.4±0.55ms     2.3 MB/sec    1.02     17.8±0.53ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.12ms     3.8 MB/sec    1.02      4.5±0.30ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   585.3±23.23µs     5.0 MB/sec    1.04   607.7±42.83µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.19ms     3.2 MB/sec    1.00      8.0±0.24ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.23ms     4.6 MB/sec    1.02      9.0±0.50ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1852.7±45.58µs     9.0 MB/sec    1.00  1860.9±115.49µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    216.0±7.02µs    13.7 MB/sec    1.00    217.0±6.78µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.12ms     6.5 MB/sec    1.02      4.0±0.20ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.10ms     3.8 MB/sec    1.02     11.1±0.15ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.06ms     7.8 MB/sec    1.01      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    238.7±5.64µs    12.4 MB/sec    1.02   244.4±10.24µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.07ms     5.5 MB/sec    1.03      4.8±0.13ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.14ms     2.7 MB/sec    1.00     15.3±0.19ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.08ms     4.1 MB/sec    1.01      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    482.9±8.42µs     6.1 MB/sec    1.02    491.7±8.01µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.09ms     3.7 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.13ms     5.1 MB/sec    1.01      8.0±0.10ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1655.0±36.11µs    10.1 MB/sec    1.00  1663.2±17.99µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.3±2.98µs    15.6 MB/sec    1.00    189.9±4.23µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.03ms     7.2 MB/sec    1.01      3.6±0.04ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-22 18:09_

---

_Closed by @charliermarsh on 2023-07-22 18:09_

---

_Comment by @charliermarsh on 2023-07-22 18:09_

Thanks!

---

_Branch deleted on 2023-07-23 00:01_

---
