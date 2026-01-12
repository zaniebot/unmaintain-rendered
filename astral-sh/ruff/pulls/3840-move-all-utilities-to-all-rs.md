```yaml
number: 3840
title: "Move `__all__` utilities to `all.rs`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/useless-ii
created_at: 2023-04-01T03:05:28Z
updated_at: 2023-04-01T03:53:48Z
url: https://github.com/astral-sh/ruff/pull/3840
synced_at: 2026-01-12T15:55:13Z
```

# Move `__all__` utilities to `all.rs`

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-01 03:31_

---

_Closed by @charliermarsh on 2023-04-01 03:31_

---

_Branch deleted on 2023-04-01 03:31_

---

_Comment by @github-actions[bot] on 2023-04-01 03:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.4±0.57ms     2.3 MB/sec    1.14     19.8±1.03ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.17ms     3.8 MB/sec    1.06      4.6±0.17ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   557.6±34.49µs     5.3 MB/sec    1.05   585.2±28.72µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.36ms     3.4 MB/sec    1.09      8.2±0.38ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.0±0.34ms     4.5 MB/sec    1.19     10.7±0.29ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1947.2±76.53µs     8.6 MB/sec    1.20      2.3±0.14ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   236.9±11.08µs    12.5 MB/sec    1.11    263.7±9.73µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.20ms     6.2 MB/sec    1.18      4.9±0.19ms     5.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     22.5±0.45ms  1847.8 KB/sec    1.00     22.5±1.03ms  1850.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.9±0.19ms     2.8 MB/sec    1.00      5.8±0.17ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   677.9±25.72µs     4.4 MB/sec    1.00   676.7±87.42µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.8±0.26ms     2.6 MB/sec    1.00      9.7±0.26ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     11.0±0.76ms     3.7 MB/sec    1.06     11.6±0.25ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.09ms     7.4 MB/sec    1.12      2.5±0.09ms     6.7 MB/sec
linter/default-rules/numpy/globals.py      1.03   298.4±11.92µs     9.9 MB/sec    1.00   288.4±15.51µs    10.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.29ms     5.2 MB/sec    1.08      5.3±0.27ms     4.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
