```yaml
number: 3887
title: Visit comprehension to detect group name usage/overrides
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/b031-false-positive
created_at: 2023-04-05T16:35:10Z
updated_at: 2023-04-06T01:23:38Z
url: https://github.com/astral-sh/ruff/pull/3887
synced_at: 2026-01-12T15:55:14Z
```

# Visit comprehension to detect group name usage/overrides

---

_@dhruvmanila_

As mentioned in the issue comment, the first commit fixes the false negative by visiting the right hand side of an assignment statement. The second commit fixes the actual bug by visiting the comprehension nodes.

fixes: #3885 

---

_Comment by @github-actions[bot] on 2023-04-05 16:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     14.7±0.92ms     2.8 MB/sec    1.00     13.8±0.30ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.10ms     4.7 MB/sec    1.00      3.5±0.10ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.04   462.4±21.19µs     6.4 MB/sec    1.00   445.1±14.58µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.20ms     4.2 MB/sec    1.02      6.1±0.29ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.26ms     5.7 MB/sec    1.00      7.1±0.18ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1646.5±77.05µs    10.1 MB/sec    1.00  1587.5±45.82µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.9±7.32µs    16.5 MB/sec    1.00    178.9±9.59µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.08ms     7.7 MB/sec    1.02      3.4±0.21ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.51ms     2.5 MB/sec    1.06     17.5±0.69ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.10ms     3.8 MB/sec    1.00      4.3±0.16ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   514.6±14.88µs     5.7 MB/sec    1.00   515.1±19.36µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.16ms     3.6 MB/sec    1.05      7.4±0.24ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      8.6±0.22ms     4.7 MB/sec    1.00      8.4±0.35ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1853.9±34.65µs     9.0 MB/sec    1.00  1768.0±23.62µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.07    207.9±7.54µs    14.2 MB/sec    1.00    195.0±6.00µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.9±0.16ms     6.5 MB/sec    1.00      3.7±0.07ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-05 22:03_

---

_Closed by @charliermarsh on 2023-04-05 22:03_

---

_Comment by @charliermarsh on 2023-04-05 22:03_

Thanks!

---

_Branch deleted on 2023-04-06 01:23_

---
