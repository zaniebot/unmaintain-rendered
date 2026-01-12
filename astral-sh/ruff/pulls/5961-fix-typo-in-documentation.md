```yaml
number: 5961
title: Fix typo in documentation
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: fix-doc-typo
created_at: 2023-07-22T01:14:36Z
updated_at: 2023-07-22T01:40:51Z
url: https://github.com/astral-sh/ruff/pull/5961
synced_at: 2026-01-12T03:30:22Z
```

# Fix typo in documentation

---

_Pull request opened by @tjkuson on 2023-07-22 01:14_

## Summary

Close unclosed inline code block that was causing the text not to render properly.

## Test Plan

`mkdocs serve`

---

_Merged by @charliermarsh on 2023-07-22 01:23_

---

_Closed by @charliermarsh on 2023-07-22 01:23_

---

_Branch deleted on 2023-07-22 01:24_

---

_Comment by @github-actions[bot] on 2023-07-22 01:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.04ms     4.4 MB/sec    1.02      9.4±0.17ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1845.3±5.58µs     9.0 MB/sec    1.01   1858.8±4.73µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    201.0±0.32µs    14.7 MB/sec    1.01    202.4±0.41µs    14.6 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.01      4.0±0.04ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.03     13.5±0.18ms     3.0 MB/sec    1.00     13.2±0.11ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.03ms     5.0 MB/sec    1.00      3.3±0.02ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    437.1±1.01µs     6.8 MB/sec    1.00    433.3±2.13µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.06ms     4.3 MB/sec    1.00      5.8±0.05ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.02ms     6.2 MB/sec    1.00      6.5±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1416.8±1.19µs    11.8 MB/sec    1.00   1410.7±3.69µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.3±0.28µs    18.6 MB/sec    1.00    157.8±0.23µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.6 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.16ms     4.1 MB/sec    1.00      9.9±0.16ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.02  1966.7±63.36µs     8.5 MB/sec    1.00  1936.8±32.23µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    221.3±8.30µs    13.3 MB/sec    1.00    220.4±7.65µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.07ms     5.9 MB/sec    1.00      4.3±0.08ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.16ms     2.9 MB/sec    1.01     14.1±0.26ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.7±0.08ms     4.5 MB/sec    1.00      3.7±0.07ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   439.9±12.09µs     6.7 MB/sec    1.00   441.7±11.95µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.4±0.14ms     4.0 MB/sec    1.00      6.3±0.11ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.11ms     5.6 MB/sec    1.00      7.3±0.08ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1502.9±28.59µs    11.1 MB/sec    1.01  1522.9±27.52µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.4±5.85µs    17.2 MB/sec    1.00    170.5±5.14µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.06ms     7.8 MB/sec    1.00      3.2±0.04ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
