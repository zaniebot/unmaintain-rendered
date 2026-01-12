```yaml
number: 5553
title: Allow MkDocs job to run on forks
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/secrets
created_at: 2023-07-06T05:38:49Z
updated_at: 2023-07-06T06:06:27Z
url: https://github.com/astral-sh/ruff/pull/5553
synced_at: 2026-01-12T15:55:18Z
```

# Allow MkDocs job to run on forks

---

_@charliermarsh_

Conditionally check whether the secret is available -- see: https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstepsif.

---

_Merged by @charliermarsh on 2023-07-06 05:46_

---

_Closed by @charliermarsh on 2023-07-06 05:46_

---

_Branch deleted on 2023-07-06 05:46_

---

_Label `documentation` added by @charliermarsh on 2023-07-06 05:47_

---

_Comment by @github-actions[bot] on 2023-07-06 05:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.04ms     5.2 MB/sec    1.00      7.9±0.04ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1753.6±3.41µs     9.5 MB/sec    1.00   1746.7±3.28µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    196.6±1.15µs    15.0 MB/sec    1.00    196.0±0.26µs    15.1 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.01ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.01     13.6±0.15ms     3.0 MB/sec    1.00     13.5±0.13ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.8±0.56µs     6.9 MB/sec    1.00    427.6±0.83µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.01      6.0±0.10ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.03ms     6.1 MB/sec    1.00      6.6±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1455.5±4.21µs    11.4 MB/sec    1.00   1440.2±2.73µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.0±0.22µs    18.0 MB/sec    1.00    163.8±0.27µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.02ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06     12.7±0.67ms     3.2 MB/sec    1.00     12.0±0.36ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.6±0.08ms     6.3 MB/sec    1.00      2.5±0.07ms     6.6 MB/sec
formatter/numpy/globals.py                 1.01    294.1±8.40µs    10.0 MB/sec    1.00    291.7±9.80µs    10.1 MB/sec
formatter/pydantic/types.py                1.04      5.7±0.14ms     4.4 MB/sec    1.00      5.5±0.11ms     4.6 MB/sec
linter/all-rules/large/dataset.py          1.07     20.8±0.74ms  2003.3 KB/sec    1.00     19.4±0.52ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.13ms     3.2 MB/sec    1.00      5.2±0.11ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.06   661.4±25.69µs     4.5 MB/sec    1.00   626.8±19.41µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.9±0.29ms     2.9 MB/sec    1.00      8.7±0.24ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.02     10.3±0.54ms     3.9 MB/sec    1.00     10.1±0.37ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     7.9 MB/sec    1.01      2.1±0.06ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.01   268.7±14.05µs    11.0 MB/sec    1.00    265.8±9.88µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.14ms     5.8 MB/sec    1.03      4.5±0.10ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
