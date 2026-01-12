```yaml
number: 5236
title: "Move `copyright` rules to `flake8_copyright` module"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/copyright
created_at: 2023-06-21T01:47:58Z
updated_at: 2023-06-21T02:20:25Z
url: https://github.com/astral-sh/ruff/pull/5236
synced_at: 2026-01-12T15:55:18Z
```

# Move `copyright` rules to `flake8_copyright` module

---

_@charliermarsh_

## Summary

I initially wanted this category to be more general and decoupled from the plugin, but I got some feedback that the titling felt inconsistent with others.


---

_Merged by @charliermarsh on 2023-06-21 01:56_

---

_Closed by @charliermarsh on 2023-06-21 01:56_

---

_Branch deleted on 2023-06-21 01:56_

---

_Comment by @github-actions[bot] on 2023-06-21 01:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.07      8.9±0.24ms     4.6 MB/sec     1.00      8.3±0.39ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.07  1852.3±117.66µs     9.0 MB/sec    1.00  1724.0±85.72µs     9.7 MB/sec
formatter/numpy/globals.py                 1.02    180.4±7.44µs    16.4 MB/sec     1.00    176.3±9.58µs    16.7 MB/sec
formatter/pydantic/types.py                1.08      3.7±0.13ms     6.9 MB/sec     1.00      3.4±0.20ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.01     17.3±0.50ms     2.4 MB/sec     1.00     17.1±0.41ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.3±0.16ms     3.9 MB/sec     1.00      4.2±0.15ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   556.8±20.80µs     5.3 MB/sec     1.00   556.7±16.65µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.6±0.29ms     3.4 MB/sec     1.00      7.5±0.22ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.19ms     4.7 MB/sec     1.00      8.7±0.27ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1894.1±57.78µs     8.8 MB/sec     1.00  1870.1±66.14µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.02   224.6±12.49µs    13.1 MB/sec     1.00    220.2±7.18µs    13.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.0±0.12ms     6.4 MB/sec     1.00      4.0±0.09ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.7±0.57ms     3.8 MB/sec    1.00     10.7±0.54ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.14ms     7.7 MB/sec    1.00      2.2±0.30ms     7.7 MB/sec
formatter/numpy/globals.py                 1.09   230.9±22.90µs    12.8 MB/sec    1.00   212.5±18.45µs    13.9 MB/sec
formatter/pydantic/types.py                1.07      4.4±0.23ms     5.8 MB/sec    1.00      4.1±0.19ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.02     21.2±1.04ms  1964.7 KB/sec    1.00     20.7±0.97ms  2009.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.6±0.30ms     3.0 MB/sec    1.01      5.6±0.28ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   657.8±44.85µs     4.5 MB/sec    1.00   656.9±40.75µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.2±0.38ms     2.8 MB/sec    1.03      9.5±0.58ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.04     11.2±0.59ms     3.6 MB/sec    1.00     10.7±0.36ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.11ms     7.3 MB/sec    1.00      2.3±0.11ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.01   277.2±15.94µs    10.6 MB/sec    1.00   275.5±19.31µs    10.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.0±0.32ms     5.1 MB/sec    1.00      5.0±0.27ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
