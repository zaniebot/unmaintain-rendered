```yaml
number: 5007
title: Add more details to E722 (bare-except) docs
type: pull_request
state: merged
author: tgross35
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2023-06-10T16:27:50Z
updated_at: 2023-06-10T22:46:59Z
url: https://github.com/astral-sh/ruff/pull/5007
synced_at: 2026-01-12T03:43:29Z
```

# Add more details to E722 (bare-except) docs

---

_Pull request opened by @tgross35 on 2023-06-10 16:27_

## Summary

Note that catching a bare `Exception` is better than catching no specific exception.

## Test Plan

Documentatino only

---

_@zanieb reviewed on 2023-06-10 16:33_

👍 nice!

---

_Comment by @github-actions[bot] on 2023-06-10 16:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.42ms     4.9 MB/sec    1.02      8.4±0.38ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1722.8±74.22µs     9.7 MB/sec    1.01  1734.4±71.64µs     9.6 MB/sec
formatter/numpy/globals.py                 1.03   174.3±13.32µs    16.9 MB/sec    1.00    169.7±9.64µs    17.4 MB/sec
formatter/pydantic/types.py                1.00      3.3±0.15ms     7.6 MB/sec    1.03      3.4±0.18ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     18.1±0.73ms     2.3 MB/sec    1.03     18.6±0.66ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.21ms     3.9 MB/sec    1.00      4.3±0.20ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   537.0±23.76µs     5.5 MB/sec    1.02   546.8±34.20µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.07      8.0±0.46ms     3.2 MB/sec    1.00      7.4±0.22ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.25ms     4.7 MB/sec    1.01      8.8±0.22ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1867.3±67.76µs     8.9 MB/sec    1.00  1858.6±55.37µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.06   223.8±10.40µs    13.2 MB/sec    1.00    210.4±7.08µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.1±0.17ms     6.2 MB/sec    1.00      3.9±0.11ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.35ms     4.2 MB/sec    1.03     10.0±0.42ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.13ms     8.2 MB/sec    1.00      2.0±0.14ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00   203.8±23.69µs    14.5 MB/sec    1.03   210.5±32.59µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.17ms     6.3 MB/sec    1.00      4.0±0.16ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.02     21.8±0.94ms  1914.3 KB/sec    1.00     21.3±0.69ms  1953.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.22ms     3.1 MB/sec    1.02      5.4±0.20ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   637.3±39.82µs     4.6 MB/sec    1.00   625.4±25.57µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.2±0.33ms     2.8 MB/sec    1.01      9.3±0.41ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.5±0.40ms     3.9 MB/sec    1.04     10.9±0.42ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.10ms     7.6 MB/sec    1.03      2.3±0.08ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   260.2±11.65µs    11.3 MB/sec    1.03   267.6±14.10µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.26ms     5.3 MB/sec    1.02      4.9±0.30ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-06-10 22:42_

---

_Merged by @charliermarsh on 2023-06-10 22:42_

---

_Closed by @charliermarsh on 2023-06-10 22:42_

---

_Comment by @charliermarsh on 2023-06-10 22:42_

Thanks :)

---

_Branch deleted on 2023-06-10 22:46_

---
