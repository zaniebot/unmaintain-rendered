```yaml
number: 4943
title: Skip class scopes when resolving nonlocal references
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/non-local
created_at: 2023-06-07T22:17:56Z
updated_at: 2023-06-07T22:49:49Z
url: https://github.com/astral-sh/ruff/pull/4943
synced_at: 2026-01-12T03:43:29Z
```

# Skip class scopes when resolving nonlocal references

---

_Pull request opened by @charliermarsh on 2023-06-07 22:17_

## Summary

When we visit a `nonlocal`, we add a usage to the resolved `nonlocal` in the parent scope. However, we're not properly skipping resolutions within class scopes (see the new snippet, below).

Closes #4940.


---

_Label `bug` added by @charliermarsh on 2023-06-07 22:18_

---

_Merged by @charliermarsh on 2023-06-07 22:25_

---

_Closed by @charliermarsh on 2023-06-07 22:25_

---

_Branch deleted on 2023-06-07 22:25_

---

_Comment by @github-actions[bot] on 2023-06-07 22:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.4±0.01ms     7.5 MB/sec    1.04      5.7±0.01ms     7.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1071.9±1.49µs    15.5 MB/sec    1.11   1190.4±1.92µs    14.0 MB/sec
formatter/numpy/globals.py                 1.00    119.6±0.13µs    24.7 MB/sec    1.16    138.4±0.32µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.4±0.02ms    10.7 MB/sec    1.06      2.5±0.00ms    10.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.03ms     2.9 MB/sec    1.06     14.8±0.02ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.03      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.1±0.72µs     7.1 MB/sec    1.00    417.7±0.77µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.04      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.0 MB/sec    1.14      7.6±0.02ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1476.0±4.68µs    11.3 MB/sec    1.08   1591.3±2.94µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.6±1.31µs    17.9 MB/sec    1.03    169.4±0.22µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.00ms     8.4 MB/sec    1.10      3.4±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.7±0.06ms     6.1 MB/sec    1.06      7.1±0.10ms     5.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1279.8±22.30µs    13.0 MB/sec    1.12  1429.0±21.79µs    11.7 MB/sec
formatter/numpy/globals.py                 1.00    141.5±3.75µs    20.9 MB/sec    1.15    162.6±4.93µs    18.2 MB/sec
formatter/pydantic/types.py                1.00      2.9±0.04ms     8.8 MB/sec    1.09      3.1±0.08ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.19ms     2.5 MB/sec    1.02     16.9±0.34ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.02      4.2±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    489.2±5.81µs     6.0 MB/sec    1.00   486.6±10.48µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.06ms     3.7 MB/sec    1.01      7.0±0.13ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.07ms     5.0 MB/sec    1.01      8.3±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1726.3±24.66µs     9.6 MB/sec    1.02  1762.9±18.30µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.6±4.21µs    14.9 MB/sec    1.01    198.9±4.55µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.06ms     7.0 MB/sec    1.03      3.8±0.04ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
