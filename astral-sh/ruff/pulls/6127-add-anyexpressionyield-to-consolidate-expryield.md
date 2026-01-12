```yaml
number: 6127
title: "Add `AnyExpressionYield` to consolidate `ExprYield` and `ExprYieldFrom`"
type: pull_request
state: merged
author: qdegraaf
labels: []
assignees: []
merged: true
base: main
head: ref/anyexpryield
created_at: 2023-07-27T15:25:13Z
updated_at: 2023-08-03T11:20:49Z
url: https://github.com/astral-sh/ruff/pull/6127
synced_at: 2026-01-12T15:55:20Z
```

# Add `AnyExpressionYield` to consolidate `ExprYield` and `ExprYieldFrom`

---

_@qdegraaf_

## Summary

Reopening of https://github.com/astral-sh/ruff/pull/6121 with a fresh fork


---

_Renamed from "Add AnyExpressionYield to consolidate ExprYield and ExprYieldFrom" to "Add `AnyExpressionYield` to consolidate `ExprYield` and `ExprYieldFrom`" by @qdegraaf on 2023-07-27 15:25_

---

_@MichaReiser approved on 2023-07-27 15:54_

Thanks, and sorry again for having to re-open this PR.

---

_Merged by @MichaReiser on 2023-07-27 16:01_

---

_Closed by @MichaReiser on 2023-07-27 16:01_

---

_Comment by @github-actions[bot] on 2023-07-27 16:20_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.4±0.40ms     3.9 MB/sec    1.02     10.6±0.36ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.1±0.06ms     8.0 MB/sec    1.00      2.1±0.08ms     8.1 MB/sec
formatter/numpy/globals.py                 1.09   236.0±11.02µs    12.5 MB/sec    1.00   216.2±13.33µs    13.6 MB/sec
formatter/pydantic/types.py                1.05      4.6±0.12ms     5.6 MB/sec    1.00      4.4±0.15ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     14.7±0.72ms     2.8 MB/sec    1.08     15.8±0.52ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.8±0.19ms     4.4 MB/sec    1.00      3.7±0.13ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   468.7±19.47µs     6.3 MB/sec    1.07   500.6±32.14µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.38ms     3.9 MB/sec    1.06      6.9±0.27ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.33ms     5.7 MB/sec    1.11      7.9±0.25ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1529.2±64.72µs    10.9 MB/sec    1.06  1628.0±50.80µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   186.0±11.80µs    15.9 MB/sec    1.01    188.1±6.39µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.09ms     7.7 MB/sec    1.08      3.6±0.18ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.6±0.21ms     3.8 MB/sec    1.09     11.6±0.26ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1996.8±40.16µs     8.3 MB/sec    1.08      2.2±0.09ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    199.0±4.36µs    14.8 MB/sec    1.08    214.4±8.28µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.12ms     5.7 MB/sec    1.08      4.8±0.12ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.24ms     2.7 MB/sec    1.01     15.2±0.22ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.22ms     4.2 MB/sec    1.00      4.0±0.10ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   387.9±17.15µs     7.6 MB/sec    1.02   396.2±17.13µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.18ms     3.7 MB/sec    1.01      6.9±0.20ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.10ms     5.2 MB/sec    1.02      7.9±0.16ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1530.2±18.47µs    10.9 MB/sec    1.02  1566.6±45.88µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    148.8±5.52µs    19.8 MB/sec    1.01    149.8±9.30µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.07ms     7.5 MB/sec    1.01      3.4±0.17ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-08-03 11:20_

---
