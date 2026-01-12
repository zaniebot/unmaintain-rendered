```yaml
number: 5304
title: Publish GitHub release as draft
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/draft
created_at: 2023-06-22T16:04:17Z
updated_at: 2023-06-22T16:31:39Z
url: https://github.com/astral-sh/ruff/pull/5304
synced_at: 2026-01-12T15:55:18Z
```

# Publish GitHub release as draft

---

_@charliermarsh_

I accidentally changed `draft: false` to `draft: true` in #5240. I actually think Copilot did this without me realizing.

---

_Merged by @charliermarsh on 2023-06-22 16:11_

---

_Closed by @charliermarsh on 2023-06-22 16:11_

---

_Branch deleted on 2023-06-22 16:11_

---

_Comment by @github-actions[bot] on 2023-06-22 16:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.01      8.0±0.05ms     5.1 MB/sec     1.00      7.9±0.03ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1804.9±132.04µs     9.2 MB/sec    1.01  1815.6±179.17µs     9.2 MB/sec
formatter/numpy/globals.py                 1.04   213.8±16.21µs    13.8 MB/sec     1.00   205.0±12.94µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.20ms     6.2 MB/sec     1.04      4.3±0.21ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.2±0.09ms     2.5 MB/sec     1.01     16.3±0.06ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.03ms     4.2 MB/sec     1.05      4.2±0.34ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    510.9±0.82µs     5.8 MB/sec     1.00    511.3±2.60µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.04ms     3.6 MB/sec     1.02      7.2±0.03ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.11ms     5.0 MB/sec     1.01      8.2±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1739.7±7.02µs     9.6 MB/sec     1.01   1750.1±4.48µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.0±0.30µs    15.1 MB/sec     1.06   206.7±13.53µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     7.0 MB/sec     1.10      4.0±0.24ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.20      9.5±0.27ms     4.3 MB/sec    1.00      7.9±0.16ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.14  1950.0±88.55µs     8.5 MB/sec    1.00  1706.9±46.72µs     9.8 MB/sec
formatter/numpy/globals.py                 1.09   207.4±11.85µs    14.2 MB/sec    1.00    190.8±8.74µs    15.5 MB/sec
formatter/pydantic/types.py                1.13      4.5±0.14ms     5.7 MB/sec    1.00      4.0±0.08ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.30ms     2.6 MB/sec    1.00     15.7±0.37ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.23ms     4.0 MB/sec    1.01      4.2±0.20ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   510.7±24.77µs     5.8 MB/sec    1.00   509.8±18.22µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.16ms     3.7 MB/sec    1.00      7.0±0.17ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.16ms     5.0 MB/sec    1.00      8.1±0.18ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1741.4±45.29µs     9.6 MB/sec    1.01  1755.8±52.67µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.01   205.5±10.09µs    14.4 MB/sec    1.00    203.5±9.09µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.09ms     6.9 MB/sec    1.00      3.7±0.09ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
