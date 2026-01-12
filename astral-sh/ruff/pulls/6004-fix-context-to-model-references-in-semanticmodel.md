```yaml
number: 6004
title: "Fix context-to-model references in `SemanticModel` documentation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/context
created_at: 2023-07-23T02:25:20Z
updated_at: 2023-07-23T02:59:31Z
url: https://github.com/astral-sh/ruff/pull/6004
synced_at: 2026-01-12T15:55:20Z
```

# Fix context-to-model references in `SemanticModel` documentation

---

_@charliermarsh_

_No description provided._

---

_Marked ready for review by @charliermarsh on 2023-07-23 02:25_

---

_Label `documentation` added by @charliermarsh on 2023-07-23 02:25_

---

_Merged by @charliermarsh on 2023-07-23 02:32_

---

_Closed by @charliermarsh on 2023-07-23 02:32_

---

_Branch deleted on 2023-07-23 02:32_

---

_Comment by @github-actions[bot] on 2023-07-23 02:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.7±0.42ms     4.2 MB/sec    1.00      9.6±0.37ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1925.3±87.03µs     8.6 MB/sec    1.00  1915.9±65.57µs     8.7 MB/sec
formatter/numpy/globals.py                 1.07   220.4±16.38µs    13.4 MB/sec    1.00    205.9±9.59µs    14.3 MB/sec
formatter/pydantic/types.py                1.01      4.3±0.23ms     6.0 MB/sec    1.00      4.2±0.21ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.38ms     3.0 MB/sec    1.01     13.5±0.44ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.11ms     4.9 MB/sec    1.01      3.5±0.10ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   454.0±19.35µs     6.5 MB/sec    1.00   447.9±18.31µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.20ms     4.2 MB/sec    1.00      6.1±0.23ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.19ms     5.9 MB/sec    1.01      6.9±0.18ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1486.2±60.46µs    11.2 MB/sec    1.01  1500.6±52.32µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.02    166.4±8.02µs    17.7 MB/sec    1.00    163.7±7.94µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.10ms     8.2 MB/sec    1.01      3.2±0.11ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     14.3±0.65ms     2.9 MB/sec    1.18     16.8±0.55ms     2.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.9±0.13ms     5.7 MB/sec    1.10      3.2±0.30ms     5.2 MB/sec
formatter/numpy/globals.py                 1.00   349.4±32.47µs     8.4 MB/sec    1.00   349.0±32.68µs     8.5 MB/sec
formatter/pydantic/types.py                1.00      6.7±0.40ms     3.8 MB/sec    1.00      6.7±0.64ms     3.8 MB/sec
linter/all-rules/large/dataset.py          1.00     20.6±0.97ms  2024.6 KB/sec    1.08     22.3±1.32ms  1866.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.26ms     3.1 MB/sec    1.11      5.9±0.32ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   612.6±27.68µs     4.8 MB/sec    1.25  765.5±119.89µs     3.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.30ms     2.9 MB/sec    1.23     10.9±1.80ms     2.3 MB/sec
linter/default-rules/large/dataset.py      1.00     10.9±0.44ms     3.7 MB/sec    1.04     11.3±1.07ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.10ms     7.4 MB/sec    1.04      2.3±0.13ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   267.4±19.80µs    11.0 MB/sec    1.09   291.4±18.32µs    10.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.20ms     5.4 MB/sec    1.10      5.2±0.28ms     4.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
