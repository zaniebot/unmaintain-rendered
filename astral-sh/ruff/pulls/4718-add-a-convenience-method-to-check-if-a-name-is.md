```yaml
number: 4718
title: Add a convenience method to check if a name is bound
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/unbound
created_at: 2023-05-30T01:38:31Z
updated_at: 2023-05-30T02:14:37Z
url: https://github.com/astral-sh/ruff/pull/4718
synced_at: 2026-01-12T03:50:03Z
```

# Add a convenience method to check if a name is bound

---

_Pull request opened by @charliermarsh on 2023-05-30 01:38_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-30 01:52_

---

_Closed by @charliermarsh on 2023-05-30 01:52_

---

_Branch deleted on 2023-05-30 01:52_

---

_Comment by @github-actions[bot] on 2023-05-30 01:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.4±0.72ms     2.1 MB/sec    1.06     20.5±0.82ms  2029.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.19ms     3.6 MB/sec    1.08      5.0±0.21ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   578.3±28.21µs     5.1 MB/sec    1.06   613.9±31.52µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.37ms     3.2 MB/sec    1.05      8.4±0.38ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.1±0.36ms     4.5 MB/sec    1.04      9.5±0.38ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.1±0.09ms     8.0 MB/sec    1.00      2.0±0.09ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.04   249.9±10.25µs    11.8 MB/sec    1.00   240.7±20.73µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.06      4.3±0.22ms     5.9 MB/sec    1.00      4.1±0.18ms     6.3 MB/sec
parser/large/dataset.py                    1.16      8.4±0.29ms     4.8 MB/sec    1.00      7.2±0.34ms     5.6 MB/sec
parser/numpy/ctypeslib.py                  1.12  1619.5±79.09µs    10.3 MB/sec    1.00  1440.1±53.40µs    11.6 MB/sec
parser/numpy/globals.py                    1.14    162.1±5.77µs    18.2 MB/sec    1.00    141.9±7.44µs    20.8 MB/sec
parser/pydantic/types.py                   1.16      3.7±0.15ms     7.0 MB/sec    1.00      3.2±0.09ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.8±0.96ms  2005.2 KB/sec    1.08     22.5±0.86ms  1853.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.19ms     3.3 MB/sec    1.14      5.8±0.24ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   616.7±27.21µs     4.8 MB/sec    1.08   666.6±33.49µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.32ms     3.0 MB/sec    1.15      9.7±0.32ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.46ms     4.0 MB/sec    1.08     10.9±0.42ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     7.8 MB/sec    1.07      2.3±0.06ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   267.8±10.66µs    11.0 MB/sec    1.05   281.3±11.31µs    10.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.26ms     5.4 MB/sec    1.04      4.9±0.16ms     5.2 MB/sec
parser/large/dataset.py                    1.00      8.4±0.31ms     4.9 MB/sec    1.00      8.4±0.33ms     4.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1585.2±56.72µs    10.5 MB/sec    1.03  1628.7±94.95µs    10.2 MB/sec
parser/numpy/globals.py                    1.00   166.9±12.43µs    17.7 MB/sec    1.02    170.2±7.55µs    17.3 MB/sec
parser/pydantic/types.py                   1.00      3.6±0.14ms     7.2 MB/sec    1.05      3.8±0.11ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
