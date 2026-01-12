```yaml
number: 5973
title: Fix typo in PYI056 docs
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-07-22T11:17:32Z
updated_at: 2023-07-22T13:10:59Z
url: https://github.com/astral-sh/ruff/pull/5973
synced_at: 2026-01-12T03:30:22Z
```

# Fix typo in PYI056 docs

---

_Pull request opened by @AlexWaygood on 2023-07-22 11:17_

The current "use instead" code would correctly be rejected by any type checker worth its salt ;)


---

_Comment by @github-actions[bot] on 2023-07-22 11:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.01      9.3±0.31ms     4.4 MB/sec     1.00      9.3±0.33ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.03  1978.2±183.87µs     8.4 MB/sec    1.00  1918.0±132.11µs     8.7 MB/sec
formatter/numpy/globals.py                 1.02   214.7±17.66µs    13.7 MB/sec     1.00   210.6±12.00µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.15ms     6.3 MB/sec     1.00      4.0±0.14ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.34ms     3.0 MB/sec     1.02     13.7±0.60ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.11ms     5.0 MB/sec     1.01      3.4±0.10ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   467.4±27.85µs     6.3 MB/sec     1.00   463.0±22.00µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.43ms     4.1 MB/sec     1.00      6.1±0.21ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.23ms     6.0 MB/sec     1.02      7.0±0.21ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1453.7±53.52µs    11.5 MB/sec     1.01  1468.0±57.16µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.9±9.57µs    17.2 MB/sec     1.01   174.4±10.18µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.11ms     8.3 MB/sec     1.01      3.1±0.10ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     14.6±0.46ms     2.8 MB/sec    1.00     14.5±0.46ms     2.8 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.8±0.15ms     5.9 MB/sec    1.00      2.7±0.12ms     6.2 MB/sec
formatter/numpy/globals.py                 1.00   320.8±10.80µs     9.2 MB/sec    1.00   319.4±16.69µs     9.2 MB/sec
formatter/pydantic/types.py                1.00      6.0±0.21ms     4.2 MB/sec    1.02      6.2±0.27ms     4.1 MB/sec
linter/all-rules/large/dataset.py          1.01     20.3±0.68ms     2.0 MB/sec    1.00     20.2±0.55ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.2±0.18ms     3.2 MB/sec    1.00      5.2±0.17ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   637.1±31.99µs     4.6 MB/sec    1.01   641.3±20.63µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.04      9.3±0.36ms     2.7 MB/sec    1.00      8.9±0.37ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01     10.8±0.35ms     3.8 MB/sec    1.00     10.7±0.33ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.6 MB/sec    1.01      2.2±0.11ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   265.8±14.79µs    11.1 MB/sec    1.00   265.0±15.12µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.7±0.25ms     5.5 MB/sec    1.00      4.6±0.16ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-07-22 13:10_

D’oh!

---

_Merged by @charliermarsh on 2023-07-22 13:10_

---

_Closed by @charliermarsh on 2023-07-22 13:10_

---

_Comment by @charliermarsh on 2023-07-22 13:10_

Thanks @AlexWaygood.

---

_Label `documentation` added by @charliermarsh on 2023-07-22 13:10_

---

_Branch deleted on 2023-07-22 13:10_

---
