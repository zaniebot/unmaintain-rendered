```yaml
number: 3942
title: "Tidy up some `pygrep-hooks` rules"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/grep-hooks
created_at: 2023-04-12T03:28:57Z
updated_at: 2023-04-12T03:56:07Z
url: https://github.com/astral-sh/ruff/pull/3942
synced_at: 2026-01-12T15:55:14Z
```

# Tidy up some `pygrep-hooks` rules

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-12 03:35_

---

_Closed by @charliermarsh on 2023-04-12 03:35_

---

_Branch deleted on 2023-04-12 03:35_

---

_Comment by @github-actions[bot] on 2023-04-12 03:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.33ms     2.5 MB/sec    1.02     16.7±0.21ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    545.3±4.03µs     5.4 MB/sec    1.04   568.6±22.33µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.09ms     3.6 MB/sec    1.02      7.2±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      8.5±0.10ms     4.8 MB/sec    1.00      8.3±0.15ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1891.8±24.01µs     8.8 MB/sec    1.00  1871.4±28.75µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    206.2±3.69µs    14.3 MB/sec    1.05    215.5±9.37µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.08ms     6.7 MB/sec    1.02      3.9±0.18ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.37ms     2.5 MB/sec    1.03     16.6±0.38ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.0 MB/sec    1.02      4.2±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    496.8±8.22µs     5.9 MB/sec    1.00   493.2±11.62µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.10ms     3.7 MB/sec    1.00      6.8±0.10ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.18ms     4.9 MB/sec    1.00      8.2±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1851.5±23.68µs     9.0 MB/sec    1.00  1829.8±23.16µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.02   199.1±11.05µs    14.8 MB/sec    1.00    194.6±4.44µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.07ms     6.7 MB/sec    1.00      3.8±0.05ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
