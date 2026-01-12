```yaml
number: 5280
title: Update reference to release step
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/yaml
created_at: 2023-06-22T01:54:53Z
updated_at: 2023-06-22T02:20:05Z
url: https://github.com/astral-sh/ruff/pull/5280
synced_at: 2026-01-12T15:55:18Z
```

# Update reference to release step

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-06-22 02:04_

---

_Closed by @charliermarsh on 2023-06-22 02:04_

---

_Branch deleted on 2023-06-22 02:04_

---

_Comment by @github-actions[bot] on 2023-06-22 02:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.05      9.5±0.28ms     4.3 MB/sec     1.00      9.1±0.40ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.04  1936.1±102.34µs     8.6 MB/sec    1.00  1869.7±75.94µs     8.9 MB/sec
formatter/numpy/globals.py                 1.01    183.9±6.44µs    16.0 MB/sec     1.00   181.6±12.40µs    16.2 MB/sec
formatter/pydantic/types.py                1.06      3.9±0.15ms     6.5 MB/sec     1.00      3.7±0.15ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     18.9±0.39ms     2.2 MB/sec     1.00     18.9±0.46ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.32ms     3.6 MB/sec     1.00      4.6±0.21ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.05   602.5±82.50µs     4.9 MB/sec     1.00   571.8±21.64µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.2±0.21ms     3.1 MB/sec     1.00      8.2±0.26ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.05      9.6±0.47ms     4.2 MB/sec     1.00      9.1±0.24ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1947.5±72.34µs     8.6 MB/sec     1.00  1907.3±62.05µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.02   227.3±10.44µs    13.0 MB/sec     1.00   222.5±12.98µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.19ms     6.2 MB/sec     1.00      4.1±0.17ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      9.6±0.38ms     4.2 MB/sec    1.00      9.4±0.35ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1959.0±86.51µs     8.5 MB/sec    1.02      2.0±0.10ms     8.3 MB/sec
formatter/numpy/globals.py                 1.04   198.2±13.05µs    14.9 MB/sec    1.00   190.8±14.26µs    15.5 MB/sec
formatter/pydantic/types.py                1.02      4.0±0.17ms     6.3 MB/sec    1.00      4.0±0.15ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     18.6±0.63ms     2.2 MB/sec    1.00     18.6±0.49ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.0±0.22ms     3.4 MB/sec    1.00      4.8±0.19ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   602.1±21.59µs     4.9 MB/sec    1.00   604.7±33.41µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.04      8.4±0.35ms     3.1 MB/sec    1.00      8.0±0.34ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.5±0.32ms     4.3 MB/sec    1.02      9.7±0.34ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.1±0.10ms     7.9 MB/sec    1.00      2.1±0.08ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.01   245.0±14.87µs    12.0 MB/sec    1.00   242.0±13.02µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.3±0.15ms     5.9 MB/sec    1.00      4.3±0.17ms     6.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
