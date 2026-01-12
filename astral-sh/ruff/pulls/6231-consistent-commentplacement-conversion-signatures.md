```yaml
number: 6231
title: "Consistent `CommentPlacement` conversion signatures"
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: formatter-comment-placement-signature
created_at: 2023-08-01T07:55:45Z
updated_at: 2023-08-01T10:01:18Z
url: https://github.com/astral-sh/ruff/pull/6231
synced_at: 2026-01-12T15:55:20Z
```

# Consistent `CommentPlacement` conversion signatures

---

_@konstin_

**Summary** Allow passing any node to `CommentPlacement::{leading, trailing, dangling}` without manually converting. Conversely, Restrict the comment to the only type we actually pass.

**Test Plan** No changes.

---

_Label `formatter` added by @konstin on 2023-08-01 07:55_

---

_Comment by @github-actions[bot] on 2023-08-01 08:27_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.03ms     4.8 MB/sec    1.00      8.4±0.08ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1650.0±33.98µs    10.1 MB/sec    1.00  1650.9±29.12µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    172.9±2.45µs    17.1 MB/sec    1.00    172.8±2.32µs    17.1 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.03ms     7.2 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     11.4±0.04ms     3.6 MB/sec    1.00     11.4±0.06ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.7 MB/sec    1.00      2.9±0.00ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    322.6±1.86µs     9.1 MB/sec    1.00    323.4±4.05µs     9.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.01ms     5.0 MB/sec    1.01      5.1±0.01ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.03ms     6.7 MB/sec    1.00      6.0±0.04ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1214.2±4.66µs    13.7 MB/sec    1.00   1201.2±6.90µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.03    128.6±8.16µs    22.9 MB/sec    1.00    125.0±6.82µs    23.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.6±0.03ms     9.6 MB/sec    1.00      2.6±0.01ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.0±0.15ms     4.1 MB/sec    1.00      9.9±0.11ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1908.0±20.34µs     8.7 MB/sec    1.00  1903.8±24.51µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    212.9±5.87µs    13.9 MB/sec    1.01   215.0±13.72µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.05ms     6.0 MB/sec    1.00      4.2±0.12ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.15ms     3.0 MB/sec    1.00     13.6±0.19ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.05ms     4.7 MB/sec    1.00      3.6±0.05ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    436.5±7.99µs     6.8 MB/sec    1.00    438.0±8.05µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.10ms     4.1 MB/sec    1.00      6.1±0.07ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.10ms     5.5 MB/sec    1.00      7.3±0.08ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1504.8±24.87µs    11.1 MB/sec    1.00  1481.4±25.55µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.5±3.21µs    17.7 MB/sec    1.00    166.1±3.52µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     7.9 MB/sec    1.00      3.2±0.04ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-08-01 09:24_

---

_Merged by @konstin on 2023-08-01 10:01_

---

_Closed by @konstin on 2023-08-01 10:01_

---

_Branch deleted on 2023-08-01 10:01_

---
