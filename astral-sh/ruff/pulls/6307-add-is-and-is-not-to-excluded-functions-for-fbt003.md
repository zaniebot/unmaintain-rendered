```yaml
number: 6307
title: "Add `is_` and `is_not` to excluded functions for `FBT003`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: FBT003/allow-is
created_at: 2023-08-03T15:27:52Z
updated_at: 2023-08-03T16:00:06Z
url: https://github.com/astral-sh/ruff/pull/6307
synced_at: 2026-01-12T02:52:04Z
```

# Add `is_` and `is_not` to excluded functions for `FBT003`

---

_Pull request opened by @zanieb on 2023-08-03 15:27_

These methods are commonly used in SQLAlchemy.

See https://github.com/astral-sh/ruff/discussions/6302


---

_@charliermarsh approved on 2023-08-03 15:40_

---

_Merged by @zanieb on 2023-08-03 15:41_

---

_Closed by @zanieb on 2023-08-03 15:41_

---

_Branch deleted on 2023-08-03 15:41_

---

_Comment by @github-actions[bot] on 2023-08-03 15:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      9.9±0.60ms     4.1 MB/sec     1.17     11.6±0.64ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1798.7±102.51µs     9.3 MB/sec    1.26      2.3±0.10ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00   211.6±16.86µs    13.9 MB/sec     1.21   255.6±23.37µs    11.5 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.18ms     6.2 MB/sec     1.16      4.8±0.24ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.10     15.1±0.72ms     2.7 MB/sec     1.00     13.7±0.64ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.10      3.8±0.27ms     4.3 MB/sec     1.00      3.5±0.21ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.12   527.9±32.77µs     5.6 MB/sec     1.00   472.8±28.80µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.11      6.9±0.45ms     3.7 MB/sec     1.00      6.2±0.51ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.18      8.9±0.49ms     4.6 MB/sec     1.00      7.5±0.39ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.19  1822.0±86.90µs     9.1 MB/sec     1.00  1531.9±82.91µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   192.2±11.59µs    15.4 MB/sec     1.00   192.5±11.02µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.11      3.8±0.30ms     6.8 MB/sec     1.00      3.4±0.14ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.09ms     4.1 MB/sec    1.00      9.9±0.08ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1915.8±23.78µs     8.7 MB/sec    1.00  1925.3±31.26µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    213.8±5.01µs    13.8 MB/sec    1.01    215.2±7.88µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.05ms     6.1 MB/sec    1.00      4.2±0.06ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.02     13.6±0.10ms     3.0 MB/sec    1.00     13.4±0.12ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.04ms     4.7 MB/sec    1.01      3.6±0.05ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    438.6±6.04µs     6.7 MB/sec    1.00    438.2±5.37µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.2±0.05ms     4.1 MB/sec    1.00      6.1±0.07ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.08ms     5.5 MB/sec    1.00      7.3±0.07ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1505.9±21.54µs    11.1 MB/sec    1.00  1488.1±17.52µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.1±3.64µs    17.7 MB/sec    1.01    169.2±3.04µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.2±0.03ms     7.9 MB/sec    1.00      3.2±0.03ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
