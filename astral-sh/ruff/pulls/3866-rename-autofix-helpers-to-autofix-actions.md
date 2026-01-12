```yaml
number: 3866
title: "Rename `autofix::helpers` to `autofix::actions`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/actions
created_at: 2023-04-03T17:16:46Z
updated_at: 2023-04-03T17:45:27Z
url: https://github.com/astral-sh/ruff/pull/3866
synced_at: 2026-01-12T04:28:19Z
```

# Rename `autofix::helpers` to `autofix::actions`

---

_Pull request opened by @charliermarsh on 2023-04-03 17:16_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-04-03 17:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.7±0.35ms     2.6 MB/sec    1.07     16.8±0.25ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.11ms     4.1 MB/sec    1.02      4.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   509.3±21.51µs     5.8 MB/sec    1.04    527.8±9.17µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.24ms     3.7 MB/sec    1.04      7.1±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.19ms     5.1 MB/sec    1.06      8.5±0.12ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1822.7±60.80µs     9.1 MB/sec    1.06  1935.3±25.88µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.7±8.15µs    14.9 MB/sec    1.09    215.9±4.31µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.09ms     6.9 MB/sec    1.07      3.9±0.06ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.0±0.15ms     2.5 MB/sec    1.00     15.7±0.12ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.07ms     4.0 MB/sec    1.00      4.1±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    503.1±7.56µs     5.9 MB/sec    1.00    498.6±7.20µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.08ms     3.7 MB/sec    1.00      6.8±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.06ms     4.9 MB/sec    1.00      8.1±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1804.4±21.22µs     9.2 MB/sec    1.00  1781.4±14.15µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    196.9±3.57µs    15.0 MB/sec    1.00    194.3±4.60µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.8 MB/sec    1.00      3.7±0.05ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-03 17:34_

---

_Closed by @charliermarsh on 2023-04-03 17:34_

---

_Branch deleted on 2023-04-03 17:34_

---
