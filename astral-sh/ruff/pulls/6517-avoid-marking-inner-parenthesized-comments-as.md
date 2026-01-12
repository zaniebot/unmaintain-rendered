```yaml
number: 6517
title: Avoid marking inner-parenthesized comments as dangling bracket comments
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/bracketed
created_at: 2023-08-12T02:00:05Z
updated_at: 2023-08-14T13:52:21Z
url: https://github.com/astral-sh/ruff/pull/6517
synced_at: 2026-01-12T02:52:04Z
```

# Avoid marking inner-parenthesized comments as dangling bracket comments

---

_Pull request opened by @charliermarsh on 2023-08-12 02:00_

## Summary

The bracketed-end-of-line comment rule is meant to assign comments like this as "immediately following the bracket":

```python
f(  # comment
    1
)
```

However, the logic was such that we treated this equivalently:

```python
f(
    (  # comment
        1
    )
)
```

This PR modifies the placement logic to ensure that we only skip the opening bracket, and not any nested brackets. The above is now formatted as:

```python
f(
    (
        # comment
        1
    )
)
```

(But will be corrected once we handle parenthesized comments properly.)

## Test Plan

`cargo test`

Confirmed no change in similarity score.


---

_Label `formatter` added by @charliermarsh on 2023-08-12 02:00_

---

_Comment by @github-actions[bot] on 2023-08-12 02:34_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.5±0.34ms     4.8 MB/sec    1.01      8.6±0.38ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.03  1777.1±61.35µs     9.4 MB/sec    1.00  1718.7±73.48µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00   193.6±10.63µs    15.2 MB/sec    1.01   196.4±11.26µs    15.0 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.16ms     7.2 MB/sec    1.00      3.6±0.17ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.03     11.2±0.46ms     3.6 MB/sec    1.00     10.9±0.37ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.11ms     5.7 MB/sec    1.01      2.9±0.12ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   420.0±21.51µs     7.0 MB/sec    1.00   422.0±15.10µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.22ms     4.5 MB/sec    1.03      5.8±0.30ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.03      5.9±0.23ms     6.9 MB/sec    1.00      5.7±0.19ms     7.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1282.1±46.19µs    13.0 MB/sec    1.01  1290.0±52.03µs    12.9 MB/sec
linter/default-rules/numpy/globals.py      1.07    155.8±5.73µs    18.9 MB/sec    1.00    146.3±7.72µs    20.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.11ms     9.7 MB/sec    1.00      2.6±0.10ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.11     14.0±0.66ms     2.9 MB/sec    1.00     12.6±0.60ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.08      2.6±0.09ms     6.3 MB/sec    1.00      2.5±0.09ms     6.8 MB/sec
formatter/numpy/globals.py                 1.05   290.8±11.01µs    10.1 MB/sec    1.00   276.5±16.45µs    10.7 MB/sec
formatter/pydantic/types.py                1.05      5.7±0.25ms     4.5 MB/sec    1.00      5.4±0.24ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.00     17.2±0.62ms     2.4 MB/sec    1.02     17.6±0.87ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.18ms     3.5 MB/sec    1.02      4.8±0.21ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   574.5±22.51µs     5.1 MB/sec    1.04   595.7±27.16µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.39ms     2.9 MB/sec    1.05      9.3±0.42ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.03      9.5±0.36ms     4.3 MB/sec    1.00      9.3±0.34ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1985.2±41.40µs     8.4 MB/sec    1.00  1925.0±92.89µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.05   245.1±15.43µs    12.0 MB/sec    1.00   234.4±13.68µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.2±0.15ms     6.0 MB/sec    1.00      4.2±0.16ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-08-14 12:10_

---

_Comment by @MichaReiser on 2023-08-14 12:10_

This attention to detail! Love it

---

_Merged by @charliermarsh on 2023-08-14 13:52_

---

_Closed by @charliermarsh on 2023-08-14 13:52_

---

_Branch deleted on 2023-08-14 13:52_

---
