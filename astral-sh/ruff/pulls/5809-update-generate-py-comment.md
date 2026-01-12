```yaml
number: 5809
title: Update generate.py comment
type: pull_request
state: merged
author: cnpryer
labels: []
assignees: []
merged: true
base: main
head: chore-fmt-docs
created_at: 2023-07-16T15:11:30Z
updated_at: 2023-07-16T18:03:44Z
url: https://github.com/astral-sh/ruff/pull/5809
synced_at: 2026-01-12T03:30:21Z
```

# Update generate.py comment

---

_Pull request opened by @cnpryer on 2023-07-16 15:11_

## Summary

The generated comment is different from the generated file's current comment.

## Test Plan

None

---

_Comment by @github-actions[bot] on 2023-07-16 15:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.10     10.8±0.02ms     3.8 MB/sec    1.00      9.8±0.02ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.09      2.0±0.00ms     8.1 MB/sec    1.00   1881.2±2.96µs     8.9 MB/sec
formatter/numpy/globals.py                 1.05    216.1±0.39µs    13.7 MB/sec    1.00    205.2±0.28µs    14.4 MB/sec
formatter/pydantic/types.py                1.11      4.6±0.01ms     5.5 MB/sec    1.00      4.2±0.00ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.8±0.03ms     2.9 MB/sec    1.01     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    381.4±0.93µs     7.7 MB/sec    1.01    384.3±1.26µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.8 MB/sec    1.02      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1473.3±6.51µs    11.3 MB/sec    1.01   1494.5±3.06µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.0±0.24µs    18.4 MB/sec    1.02    163.8±0.30µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.1 MB/sec    1.02      3.2±0.01ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.8±0.77ms     3.5 MB/sec    1.10     13.0±0.82ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.5±0.23ms     6.7 MB/sec    1.00      2.5±0.17ms     6.7 MB/sec
formatter/numpy/globals.py                 1.04   290.4±23.73µs    10.2 MB/sec    1.00   280.3±31.22µs    10.5 MB/sec
formatter/pydantic/types.py                1.11      5.6±0.37ms     4.6 MB/sec    1.00      5.0±0.37ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.08     19.8±0.95ms     2.1 MB/sec    1.00     18.3±1.33ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.19      5.3±0.22ms     3.1 MB/sec    1.00      4.5±0.22ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.17   658.8±44.70µs     4.5 MB/sec    1.00   565.3±44.59µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.14      9.2±0.36ms     2.8 MB/sec    1.00      8.0±0.64ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.73ms     4.1 MB/sec    1.03     10.1±0.68ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04      2.1±0.07ms     8.0 MB/sec    1.00  1993.9±156.05µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.07   249.7±13.00µs    11.8 MB/sec    1.00   233.3±14.20µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.11      4.6±0.29ms     5.6 MB/sec    1.00      4.1±0.26ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-16 15:51_

---

_Closed by @charliermarsh on 2023-07-16 15:51_

---

_Comment by @charliermarsh on 2023-07-16 15:51_

Thanks!

---

_Branch deleted on 2023-07-16 17:12_

---
