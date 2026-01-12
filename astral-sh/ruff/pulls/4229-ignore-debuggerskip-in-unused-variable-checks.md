```yaml
number: 4229
title: Ignore __debuggerskip__ in unused variable checks
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/debugger
created_at: 2023-05-04T19:39:38Z
updated_at: 2023-05-04T20:15:27Z
url: https://github.com/astral-sh/ruff/pull/4229
synced_at: 2026-01-12T04:28:19Z
```

# Ignore __debuggerskip__ in unused variable checks

---

_Pull request opened by @charliermarsh on 2023-05-04 19:39_

## Summary

This is used in [IPython](https://ipython.readthedocs.io/en/7.31.0/whatsnew/version7.html#ipython-7-29) to tell `pdb` to avoid stepping into certain functions, like decorators.

Closes #4228.


---

_Merged by @charliermarsh on 2023-05-04 19:45_

---

_Closed by @charliermarsh on 2023-05-04 19:45_

---

_Branch deleted on 2023-05-04 19:45_

---

_Comment by @github-actions[bot] on 2023-05-04 19:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.7±0.15ms     2.4 MB/sec    1.00     16.6±0.11ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.00      4.0±0.04ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    492.9±5.78µs     6.0 MB/sec    1.03   506.0±12.97µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.01      7.0±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.06ms     4.9 MB/sec    1.00      8.2±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1775.6±14.08µs     9.4 MB/sec    1.00  1765.2±20.55µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.4±3.12µs    14.8 MB/sec    1.00    199.2±2.97µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.04ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.5±0.04ms     6.3 MB/sec    1.00      6.5±0.05ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1258.2±12.83µs    13.2 MB/sec    1.00   1262.3±9.63µs    13.2 MB/sec
parser/numpy/globals.py                    1.01    129.8±1.05µs    22.7 MB/sec    1.00    128.3±1.51µs    23.0 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.2 MB/sec    1.00      2.8±0.02ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     23.5±1.41ms  1774.5 KB/sec    1.00     22.8±0.85ms  1829.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.36ms     2.9 MB/sec    1.03      5.8±0.28ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   642.8±39.28µs     4.6 MB/sec    1.00   639.4±38.57µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.3±0.40ms     2.7 MB/sec    1.05      9.7±0.50ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     11.1±0.39ms     3.7 MB/sec    1.04     11.6±0.95ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.09ms     7.1 MB/sec    1.03      2.4±0.15ms     6.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   286.1±14.83µs    10.3 MB/sec    1.02   290.4±19.72µs    10.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      5.2±0.26ms     4.9 MB/sec    1.00      5.1±0.19ms     5.0 MB/sec
parser/large/dataset.py                    1.00      9.0±0.29ms     4.5 MB/sec    1.00      9.0±0.34ms     4.5 MB/sec
parser/numpy/ctypeslib.py                  1.00  1737.0±64.39µs     9.6 MB/sec    1.01  1746.6±92.20µs     9.5 MB/sec
parser/numpy/globals.py                    1.00    173.5±7.53µs    17.0 MB/sec    1.01   175.8±10.66µs    16.8 MB/sec
parser/pydantic/types.py                   1.00      3.9±0.14ms     6.6 MB/sec    1.00      3.9±0.12ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
