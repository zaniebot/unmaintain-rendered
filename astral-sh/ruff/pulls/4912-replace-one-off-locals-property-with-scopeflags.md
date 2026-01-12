```yaml
number: 4912
title: "Replace one-off locals property with `ScopeFlags`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/scope-flags
created_at: 2023-06-07T01:11:46Z
updated_at: 2023-06-07T01:37:28Z
url: https://github.com/astral-sh/ruff/pull/4912
synced_at: 2026-01-12T03:43:29Z
```

# Replace one-off locals property with `ScopeFlags`

---

_Pull request opened by @charliermarsh on 2023-06-07 01:11_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-07 01:11_

---

_Merged by @charliermarsh on 2023-06-07 01:22_

---

_Closed by @charliermarsh on 2023-06-07 01:22_

---

_Branch deleted on 2023-06-07 01:22_

---

_Comment by @github-actions[bot] on 2023-06-07 01:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.9±0.02ms     5.9 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.01   1437.4±3.36µs    11.6 MB/sec    1.00   1428.0±3.29µs    11.7 MB/sec
formatter/numpy/globals.py                 1.05    175.4±8.10µs    16.8 MB/sec    1.00    166.9±1.94µs    17.7 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
linter/all-rules/large/dataset.py          1.01     17.0±0.29ms     2.4 MB/sec    1.00     16.8±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.04ms     4.1 MB/sec    1.00      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    493.8±0.65µs     6.0 MB/sec    1.00    490.0±0.60µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.03ms     3.6 MB/sec    1.00      7.0±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.1±0.03ms     5.0 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1736.9±7.05µs     9.6 MB/sec    1.00   1717.7±1.49µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.09    202.7±8.19µs    14.6 MB/sec    1.00    186.8±0.32µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.7±0.25ms     6.8 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.1±0.39ms     4.5 MB/sec    1.00      9.1±0.43ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1796.7±64.17µs     9.3 MB/sec    1.00  1796.9±100.28µs     9.3 MB/sec
formatter/numpy/globals.py                 1.01   209.6±12.97µs    14.1 MB/sec    1.00   208.5±15.90µs    14.1 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.16ms     6.4 MB/sec    1.00      3.9±0.23ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     21.5±0.85ms  1934.0 KB/sec    1.02     21.9±0.85ms  1900.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.6±0.29ms     3.0 MB/sec    1.03      5.8±0.37ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   624.7±32.44µs     4.7 MB/sec    1.00   621.9±28.14µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.38ms     2.8 MB/sec    1.02      9.2±0.44ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.35ms     3.9 MB/sec    1.02     10.5±0.36ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.3±0.09ms     7.4 MB/sec    1.00      2.2±0.09ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.01   262.5±35.48µs    11.2 MB/sec    1.00   260.6±12.58µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.18ms     5.3 MB/sec    1.01      4.8±0.25ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
