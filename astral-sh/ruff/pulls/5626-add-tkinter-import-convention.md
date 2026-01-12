```yaml
number: 5626
title: "Add `tkinter` import convention"
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: tk
created_at: 2023-07-09T10:14:19Z
updated_at: 2023-07-09T11:13:14Z
url: https://github.com/astral-sh/ruff/pull/5626
synced_at: 2026-01-12T15:55:19Z
```

# Add `tkinter` import convention

---

_@tjkuson_

## Summary

Adds `import tkinter as tk` to the list of default import conventions.

Closes #5620.

## Test Plan

Added `tkinter` to test fixture.

`cargo test`


---

_Renamed from "Add tkinter import convention" to "Add `tkinter` import convention" by @tjkuson on 2023-07-09 10:14_

---

_Comment by @github-actions[bot] on 2023-07-09 10:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.09     10.6±0.45ms     3.9 MB/sec    1.00      9.7±0.38ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.10      2.3±0.13ms     7.4 MB/sec    1.00      2.1±0.10ms     8.1 MB/sec
formatter/numpy/globals.py                 1.10   269.5±13.32µs    10.9 MB/sec    1.00   244.4±17.43µs    12.1 MB/sec
formatter/pydantic/types.py                1.12      5.2±0.30ms     4.9 MB/sec    1.00      4.6±0.26ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     16.7±0.85ms     2.4 MB/sec    1.01     16.8±0.74ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.20ms     4.0 MB/sec    1.00      4.1±0.20ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   528.0±37.97µs     5.6 MB/sec    1.06   559.7±41.85µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.59ms     3.5 MB/sec    1.04      7.5±0.35ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.39ms     5.0 MB/sec    1.01      8.3±0.46ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1747.4±90.84µs     9.5 MB/sec    1.02  1787.1±74.10µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   216.7±15.14µs    13.6 MB/sec    1.00   216.9±11.67µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.18ms     7.0 MB/sec    1.05      3.9±0.16ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.07ms     4.4 MB/sec    1.03      9.5±0.38ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.03ms     8.3 MB/sec    1.00      2.0±0.03ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    229.0±3.63µs    12.9 MB/sec    1.01   230.5±10.18µs    12.8 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.05ms     5.7 MB/sec    1.01      4.5±0.08ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.09ms     2.6 MB/sec    1.01     15.5±0.15ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.1 MB/sec    1.01      4.1±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    502.1±7.23µs     5.9 MB/sec    1.00    503.4±9.21µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.00      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.07ms     5.1 MB/sec    1.01      8.0±0.13ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1701.3±14.82µs     9.8 MB/sec    1.02  1727.1±37.43µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.3±3.30µs    14.5 MB/sec    1.00    204.1±3.90µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.2 MB/sec    1.03      3.7±0.13ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@dhruvmanila approved on 2023-07-09 10:51_

---

_Merged by @dhruvmanila on 2023-07-09 10:56_

---

_Closed by @dhruvmanila on 2023-07-09 10:56_

---

_Comment by @dhruvmanila on 2023-07-09 10:56_

Thanks!

---

_Branch deleted on 2023-07-09 11:13_

---
