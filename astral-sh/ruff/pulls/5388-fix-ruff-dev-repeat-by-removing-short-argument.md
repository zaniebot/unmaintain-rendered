```yaml
number: 5388
title: Fix ruff_dev repeat by removing short argument
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: fix_ruff_dev_repeat
created_at: 2023-06-27T13:22:42Z
updated_at: 2023-06-27T13:48:44Z
url: https://github.com/astral-sh/ruff/pull/5388
synced_at: 2026-01-12T03:36:55Z
```

# Fix ruff_dev repeat by removing short argument

---

_Pull request opened by @konstin on 2023-06-27 13:22_

ruff_dev repeat recently broke (i think with the cargo update?):

> thread 'main' panicked at 'Command repeat: Short option names must be unique for each argument, but '-n' is in use by both 'no_cache' and 'repeat''

This fixes this by removing the short argument.


---

_Label `internal` added by @konstin on 2023-06-27 13:22_

---

_@charliermarsh approved on 2023-06-27 13:23_

---

_Merged by @konstin on 2023-06-27 13:29_

---

_Closed by @konstin on 2023-06-27 13:29_

---

_Branch deleted on 2023-06-27 13:29_

---

_Comment by @github-actions[bot] on 2023-06-27 13:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.6±0.02ms     4.7 MB/sec    1.00      8.5±0.02ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01   1809.6±2.48µs     9.2 MB/sec    1.00   1787.9±3.05µs     9.3 MB/sec
formatter/numpy/globals.py                 1.00    196.7±1.02µs    15.0 MB/sec    1.00    195.8±0.42µs    15.1 MB/sec
formatter/pydantic/types.py                1.02      4.0±0.01ms     6.4 MB/sec    1.00      3.9±0.00ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.06ms     3.1 MB/sec    1.01     13.1±0.15ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.6±0.66µs     7.0 MB/sec    1.00    424.7±0.78µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.05ms     4.4 MB/sec    1.00      5.8±0.05ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.2 MB/sec    1.00      6.6±0.05ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1450.7±2.74µs    11.5 MB/sec    1.00   1447.3±6.21µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.8±0.32µs    18.1 MB/sec    1.00    162.5±0.37µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.5±0.09ms     4.3 MB/sec    1.00      9.4±0.08ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.02ms     8.3 MB/sec    1.00  1993.7±67.52µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    227.0±4.91µs    13.0 MB/sec    1.01    229.9±9.53µs    12.8 MB/sec
formatter/pydantic/types.py                1.01      4.4±0.06ms     5.8 MB/sec    1.00      4.4±0.06ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.13ms     2.7 MB/sec    1.00     15.2±0.18ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.00      4.1±0.07ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    503.9±7.95µs     5.9 MB/sec    1.00   499.5±15.81µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.07ms     3.7 MB/sec    1.00      6.7±0.06ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.10ms     5.2 MB/sec    1.00      7.9±0.06ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1697.2±21.51µs     9.8 MB/sec    1.00  1683.1±15.36µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    197.2±4.17µs    15.0 MB/sec    1.00    195.2±2.97µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.1 MB/sec    1.00      3.6±0.04ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
