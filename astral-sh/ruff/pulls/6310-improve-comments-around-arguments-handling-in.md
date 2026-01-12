```yaml
number: 6310
title: "Improve comments around `Arguments` handling in classes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/decorators
created_at: 2023-08-03T16:01:33Z
updated_at: 2023-08-03T16:42:09Z
url: https://github.com/astral-sh/ruff/pull/6310
synced_at: 2026-01-12T15:55:21Z
```

# Improve comments around `Arguments` handling in classes

---

_@charliermarsh_

## Summary

Based on the confusion here: https://github.com/astral-sh/ruff/pull/6274#discussion_r1282754515.

I looked into moving this logic into `placement.rs`, but I think it's trickier than it may appear.


---

_Label `internal` added by @charliermarsh on 2023-08-03 16:01_

---

_Converted to draft by @charliermarsh on 2023-08-03 16:02_

---

_Comment by @charliermarsh on 2023-08-03 16:02_

Will make one more attempt to move to `placement.rs`.

---

_Marked ready for review by @charliermarsh on 2023-08-03 16:30_

---

_Comment by @charliermarsh on 2023-08-03 16:31_

I think this is ultimately simpler than adding cases to `placement.rs`.

---

_Renamed from "Improve comments around Arguments handling in classes" to "Improve comments around `Arguments` handling in classes" by @charliermarsh on 2023-08-03 16:31_

---

_Merged by @charliermarsh on 2023-08-03 16:34_

---

_Closed by @charliermarsh on 2023-08-03 16:34_

---

_Branch deleted on 2023-08-03 16:34_

---

_Comment by @github-actions[bot] on 2023-08-03 16:42_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.06ms     4.2 MB/sec    1.02      9.9±0.15ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1930.7±9.09µs     8.6 MB/sec    1.00   1939.2±9.52µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    215.5±2.60µs    13.7 MB/sec    1.00    215.8±1.72µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.03ms     6.2 MB/sec    1.00      4.2±0.03ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.2±0.07ms     3.1 MB/sec    1.03     13.6±0.48ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.00      3.4±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    457.1±3.03µs     6.5 MB/sec    1.01    460.1±2.36µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.3 MB/sec    1.00      6.0±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.04ms     6.0 MB/sec    1.01      6.9±0.04ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1418.1±6.85µs    11.7 MB/sec    1.02  1442.7±29.43µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.3±1.47µs    18.9 MB/sec    1.02    159.5±0.54µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.02ms     8.5 MB/sec    1.01      3.0±0.02ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     14.8±0.61ms     2.8 MB/sec    1.25     18.4±1.02ms     2.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.7±0.13ms     6.1 MB/sec    1.18      3.2±0.16ms     5.1 MB/sec
formatter/numpy/globals.py                 1.00   302.6±24.05µs     9.8 MB/sec    1.20   362.9±36.79µs     8.1 MB/sec
formatter/pydantic/types.py                1.00      6.1±0.29ms     4.2 MB/sec    1.26      7.6±0.46ms     3.3 MB/sec
linter/all-rules/large/dataset.py          1.03     20.4±0.80ms  2039.6 KB/sec    1.00     19.8±0.89ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.2±0.30ms     3.2 MB/sec    1.00      5.1±0.28ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.02   629.2±43.04µs     4.7 MB/sec    1.00   617.8±44.65µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.1±0.52ms     2.8 MB/sec    1.00      9.1±0.50ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.08     11.5±0.65ms     3.5 MB/sec    1.00     10.7±0.57ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05      2.2±0.10ms     7.7 MB/sec    1.00      2.1±0.09ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.02   267.2±14.41µs    11.0 MB/sec    1.00   261.3±20.61µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.07      4.8±0.24ms     5.3 MB/sec    1.00      4.5±0.21ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
