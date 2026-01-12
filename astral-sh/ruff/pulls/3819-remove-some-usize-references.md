```yaml
number: 3819
title: "Remove some `usize` references"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/usize
created_at: 2023-03-30T17:22:05Z
updated_at: 2023-03-30T21:35:43Z
url: https://github.com/astral-sh/ruff/pull/3819
synced_at: 2026-01-12T15:55:13Z
```

# Remove some `usize` references

---

_@charliermarsh_

_No description provided._

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-30 17:22_

---

_Comment by @github-actions[bot] on 2023-03-30 17:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.47ms     2.4 MB/sec    1.00     17.1±0.49ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.16ms     3.8 MB/sec    1.00      4.3±0.15ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   566.7±20.85µs     5.2 MB/sec    1.01   570.3±23.21µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.21ms     3.5 MB/sec    1.00      7.4±0.25ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.27ms     4.7 MB/sec    1.00      8.7±0.28ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1985.8±87.28µs     8.4 MB/sec    1.00  1901.7±47.01µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   240.1±10.85µs    12.3 MB/sec    1.00   240.0±11.21µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.13ms     6.4 MB/sec    1.00      4.0±0.16ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.4±0.13ms     3.0 MB/sec    1.01     13.5±0.20ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.04ms     4.9 MB/sec    1.00      3.3±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    356.1±3.19µs     8.3 MB/sec    1.00    356.0±7.51µs     8.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.09ms     4.4 MB/sec    1.00      5.7±0.10ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.14ms     5.6 MB/sec    1.00      7.2±0.06ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1495.6±8.97µs    11.1 MB/sec    1.00  1481.1±14.08µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.1±1.25µs    18.9 MB/sec    1.00    156.9±5.29µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.02ms     8.0 MB/sec    1.00      3.2±0.07ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-03-30 21:33_

---

_Merged by @charliermarsh on 2023-03-30 21:35_

---

_Closed by @charliermarsh on 2023-03-30 21:35_

---

_Branch deleted on 2023-03-30 21:35_

---
