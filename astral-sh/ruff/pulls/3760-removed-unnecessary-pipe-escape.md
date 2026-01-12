```yaml
number: 3760
title: Removed unnecessary pipe escape
type: pull_request
state: merged
author: trag1c
labels: []
assignees: []
merged: true
base: main
head: md-table-pipe-fix
created_at: 2023-03-27T17:33:46Z
updated_at: 2023-03-27T18:14:39Z
url: https://github.com/astral-sh/ruff/pull/3760
synced_at: 2026-01-12T04:39:45Z
```

# Removed unnecessary pipe escape

---

_Pull request opened by @trag1c on 2023-03-27 17:33_

It seems like https://github.com/squidfunk/mkdocs-material handles `|`s in code blocks automatically so there's no need to escape them for table columns.

---

_Merged by @charliermarsh on 2023-03-27 17:49_

---

_Closed by @charliermarsh on 2023-03-27 17:49_

---

_Comment by @charliermarsh on 2023-03-27 17:49_

Thank you!

---

_Comment by @github-actions[bot] on 2023-03-27 17:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.02ms     2.9 MB/sec    1.00     14.2±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.00ms     4.4 MB/sec    1.00      3.7±0.00ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    432.3±1.86µs     6.8 MB/sec    1.00    432.0±1.17µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.0 MB/sec    1.00      6.3±0.02ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.01ms     5.3 MB/sec    1.00      7.7±0.01ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1685.0±4.22µs     9.9 MB/sec    1.00  1671.8±11.38µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.7±0.30µs    16.8 MB/sec    1.01    176.7±0.67µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.12ms     2.7 MB/sec    1.00     15.1±0.11ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.1 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    525.3±3.78µs     5.6 MB/sec    1.00    524.4±3.93µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.05ms     3.8 MB/sec    1.01      6.7±0.10ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.04ms     5.0 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1808.1±34.19µs     9.2 MB/sec    1.00  1806.8±14.44µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.8±3.16µs    14.6 MB/sec    1.00    202.7±5.83µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.7 MB/sec    1.00      3.8±0.05ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-03-27 18:14_

---
