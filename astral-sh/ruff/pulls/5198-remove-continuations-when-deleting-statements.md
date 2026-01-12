```yaml
number: 5198
title: Remove continuations when deleting statements
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/continuations
created_at: 2023-06-20T01:43:19Z
updated_at: 2023-06-20T02:25:39Z
url: https://github.com/astral-sh/ruff/pull/5198
synced_at: 2026-01-12T15:55:17Z
```

# Remove continuations when deleting statements

---

_@charliermarsh_

## Summary

This PR modifies our statement deletion logic to delete any preceding continuation lines.

For example, given:

```py
x = 1; \
  import os
```

We'll now rewrite to:

```py
x = 1;
```

In addition, the logic can now handle multiple preceding continuations (which is unlikely, but valid).


---

_Comment by @github-actions[bot] on 2023-06-20 02:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1381.2±1.83µs    12.1 MB/sec    1.01   1390.1±1.85µs    12.0 MB/sec
formatter/numpy/globals.py                 1.00    135.8±0.27µs    21.7 MB/sec    1.00    135.7±0.91µs    21.7 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.03ms     9.3 MB/sec    1.00      2.8±0.02ms     9.3 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.05ms     2.9 MB/sec    1.00     14.3±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    367.4±1.19µs     8.0 MB/sec    1.00    368.9±1.49µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.02ms     5.5 MB/sec    1.00      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1557.7±3.03µs    10.7 MB/sec    1.00   1561.2±4.17µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.7±0.26µs    17.6 MB/sec    1.00    168.3±5.15µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.22ms     5.1 MB/sec    1.00      7.8±0.03ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1590.4±18.19µs    10.5 MB/sec    1.00  1596.2±17.11µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00   154.3±11.24µs    19.1 MB/sec    1.01    155.6±3.70µs    19.0 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.03ms     8.0 MB/sec    1.01      3.2±0.03ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.18ms     2.5 MB/sec    1.01     16.1±0.23ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.0 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    426.5±6.69µs     6.9 MB/sec    1.02    433.6±8.56µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.06ms     3.6 MB/sec    1.00      7.0±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.06ms     4.8 MB/sec    1.00      8.4±0.13ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1766.2±17.93µs     9.4 MB/sec    1.00  1763.3±16.88µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.4±2.25µs    15.7 MB/sec    1.00    187.2±2.06µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.7 MB/sec    1.01      3.8±0.03ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-20 02:04_

---

_Closed by @charliermarsh on 2023-06-20 02:04_

---

_Branch deleted on 2023-06-20 02:04_

---
