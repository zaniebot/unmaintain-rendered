```yaml
number: 5142
title: Use const-singleton helpers in more rules
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/const
created_at: 2023-06-16T04:22:07Z
updated_at: 2023-06-16T04:48:39Z
url: https://github.com/astral-sh/ruff/pull/5142
synced_at: 2026-01-12T03:43:30Z
```

# Use const-singleton helpers in more rules

---

_Pull request opened by @charliermarsh on 2023-06-16 04:22_

_No description provided._

---

_Merged by @charliermarsh on 2023-06-16 04:28_

---

_Closed by @charliermarsh on 2023-06-16 04:28_

---

_Branch deleted on 2023-06-16 04:28_

---

_Comment by @github-actions[bot] on 2023-06-16 04:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.07      7.7±0.52ms     5.3 MB/sec     1.00      7.2±0.54ms     5.7 MB/sec
formatter/numpy/ctypeslib.py               1.06  1559.6±137.09µs    10.7 MB/sec    1.00  1476.7±121.49µs    11.3 MB/sec
formatter/numpy/globals.py                 1.00   156.2±15.18µs    18.9 MB/sec     1.00   156.5±22.20µs    18.9 MB/sec
formatter/pydantic/types.py                1.02      3.1±0.27ms     8.3 MB/sec     1.00      3.0±0.23ms     8.5 MB/sec
linter/all-rules/large/dataset.py          1.04     16.5±1.10ms     2.5 MB/sec     1.00     15.9±0.93ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.0±0.24ms     4.2 MB/sec     1.00      3.9±0.25ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   485.6±38.13µs     6.1 MB/sec     1.01   489.5±40.07µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.48ms     3.7 MB/sec     1.00      6.8±0.46ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.06      8.0±0.45ms     5.1 MB/sec     1.00      7.5±0.43ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1719.1±129.81µs     9.7 MB/sec    1.00  1641.1±136.24µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   196.2±15.67µs    15.0 MB/sec     1.00   196.8±20.35µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.6±0.27ms     7.1 MB/sec     1.00      3.5±0.26ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.1±0.21ms     5.7 MB/sec    1.00      7.0±0.15ms     5.8 MB/sec
formatter/numpy/ctypeslib.py               1.02  1437.5±31.12µs    11.6 MB/sec    1.00  1410.6±32.85µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    134.2±3.87µs    22.0 MB/sec    1.02    137.3±7.51µs    21.5 MB/sec
formatter/pydantic/types.py                1.00      2.9±0.08ms     8.8 MB/sec    1.00      2.9±0.14ms     8.8 MB/sec
linter/all-rules/large/dataset.py          1.02     15.4±0.40ms     2.6 MB/sec    1.00     15.1±0.33ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.07ms     4.4 MB/sec    1.00      3.8±0.07ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.02   461.3±20.18µs     6.4 MB/sec    1.00   452.9±19.35µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.14ms     3.9 MB/sec    1.00      6.5±0.18ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.02      7.6±0.16ms     5.4 MB/sec    1.00      7.5±0.13ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1602.2±35.99µs    10.4 MB/sec    1.00  1576.9±45.74µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.0±5.43µs    16.6 MB/sec    1.00    177.8±4.68µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.11ms     7.4 MB/sec    1.02      3.5±0.29ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
