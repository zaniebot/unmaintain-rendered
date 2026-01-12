```yaml
number: 4868
title: "Mark F523 as \"sometimes\" fixable"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/sometimes
created_at: 2023-06-05T16:45:45Z
updated_at: 2023-06-05T17:20:44Z
url: https://github.com/astral-sh/ruff/pull/4868
synced_at: 2026-01-12T15:55:16Z
```

# Mark F523 as "sometimes" fixable

---

_@charliermarsh_

Closes #4865.

---

_Merged by @charliermarsh on 2023-06-05 16:55_

---

_Closed by @charliermarsh on 2023-06-05 16:55_

---

_Branch deleted on 2023-06-05 16:55_

---

_Comment by @github-actions[bot] on 2023-06-05 16:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     17.6±0.48ms     2.3 MB/sec    1.00     16.9±0.35ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.2±0.16ms     4.0 MB/sec    1.00      4.1±0.12ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   519.6±25.60µs     5.7 MB/sec    1.00   513.1±25.12µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.2±0.14ms     3.5 MB/sec    1.00      7.0±0.17ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.03      8.3±0.19ms     4.9 MB/sec    1.00      8.1±0.20ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1787.2±56.81µs     9.3 MB/sec    1.00  1769.3±42.74µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.5±8.32µs    14.6 MB/sec    1.04    210.2±9.36µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.11ms     6.8 MB/sec    1.00      3.7±0.12ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.5±0.11ms     6.3 MB/sec    1.00      6.4±0.17ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.01  1295.3±55.69µs    12.9 MB/sec    1.00  1277.0±34.94µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    129.8±5.99µs    22.7 MB/sec    1.02    132.7±5.73µs    22.2 MB/sec
parser/pydantic/types.py                   1.03      2.8±0.07ms     9.0 MB/sec    1.00      2.8±0.08ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.2±0.64ms  1967.5 KB/sec    1.04     22.0±0.74ms  1897.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.4±0.21ms     3.1 MB/sec    1.00      5.2±0.19ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   614.2±24.26µs     4.8 MB/sec    1.00   616.4±24.63µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.31ms     2.9 MB/sec    1.01      9.0±0.30ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.02     10.7±0.35ms     3.8 MB/sec    1.00     10.6±0.36ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.5 MB/sec    1.01      2.2±0.07ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   261.2±14.15µs    11.3 MB/sec    1.00   261.4±10.52µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.8±0.17ms     5.3 MB/sec    1.00      4.8±0.16ms     5.4 MB/sec
parser/large/dataset.py                    1.00      8.4±0.14ms     4.8 MB/sec    1.03      8.6±0.31ms     4.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1599.9±66.86µs    10.4 MB/sec    1.04  1666.4±49.63µs    10.0 MB/sec
parser/numpy/globals.py                    1.00    161.2±5.16µs    18.3 MB/sec    1.05    169.5±6.78µs    17.4 MB/sec
parser/pydantic/types.py                   1.00      3.7±0.09ms     7.0 MB/sec    1.05      3.9±0.13ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
