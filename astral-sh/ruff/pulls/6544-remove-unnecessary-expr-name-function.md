```yaml
number: 6544
title: "Remove unnecessary `expr_name` function"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/expr-name
created_at: 2023-08-14T03:30:05Z
updated_at: 2023-08-14T04:04:13Z
url: https://github.com/astral-sh/ruff/pull/6544
synced_at: 2026-01-12T15:55:21Z
```

# Remove unnecessary `expr_name` function

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-08-14 03:30_

---

_Comment by @github-actions[bot] on 2023-08-14 03:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.04ms     4.9 MB/sec    1.00      8.3±0.03ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.01  1687.8±48.26µs     9.9 MB/sec    1.00  1671.4±46.78µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    189.9±7.72µs    15.5 MB/sec    1.00    189.0±7.16µs    15.6 MB/sec
formatter/pydantic/types.py                1.02      3.5±0.15ms     7.2 MB/sec    1.00      3.5±0.03ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.04ms     3.9 MB/sec    1.01     10.5±0.03ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.8±0.02ms     5.9 MB/sec    1.00      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    397.4±2.06µs     7.4 MB/sec    1.00    396.6±1.15µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.02ms     4.7 MB/sec    1.00      5.4±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.4 MB/sec    1.01      5.5±0.02ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1210.0±8.05µs    13.8 MB/sec    1.01   1216.7±5.50µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    141.8±0.87µs    20.8 MB/sec    1.00    141.7±0.25µs    20.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.3 MB/sec    1.01      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.1±0.38ms     3.4 MB/sec    1.00     12.1±0.39ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.4±0.10ms     7.0 MB/sec    1.00      2.3±0.11ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00   266.2±35.53µs    11.1 MB/sec    1.01   268.6±19.52µs    11.0 MB/sec
formatter/pydantic/types.py                1.00      5.1±0.22ms     5.0 MB/sec    1.00      5.1±0.21ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.02     15.7±0.44ms     2.6 MB/sec    1.00     15.4±0.42ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.28ms     3.9 MB/sec    1.00      4.3±0.15ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02   550.3±28.14µs     5.4 MB/sec    1.00   539.9±22.54µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.28ms     3.2 MB/sec    1.02      8.2±0.34ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.19ms     4.8 MB/sec    1.00      8.4±0.21ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1809.0±59.98µs     9.2 MB/sec    1.00  1794.2±63.81µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.02   220.1±11.08µs    13.4 MB/sec    1.00   216.7±10.04µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.12ms     6.7 MB/sec    1.00      3.8±0.13ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-14 03:51_

---

_Closed by @charliermarsh on 2023-08-14 03:51_

---

_Branch deleted on 2023-08-14 03:51_

---
