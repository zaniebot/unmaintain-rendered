```yaml
number: 4129
title: Preserve star-handling special-casing for force-single-line
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/star-force
created_at: 2023-04-27T03:56:22Z
updated_at: 2023-04-27T04:24:18Z
url: https://github.com/astral-sh/ruff/pull/4129
synced_at: 2026-01-12T15:55:14Z
```

# Preserve star-handling special-casing for force-single-line

---

_@charliermarsh_

Closes #4118 (though I want to do another pass over this logic to clean it up).


---

_Merged by @charliermarsh on 2023-04-27 04:02_

---

_Closed by @charliermarsh on 2023-04-27 04:02_

---

_Branch deleted on 2023-04-27 04:02_

---

_Comment by @github-actions[bot] on 2023-04-27 04:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.8±0.34ms     2.6 MB/sec    1.02     16.1±1.11ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.09ms     4.5 MB/sec    1.03      3.8±0.12ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   472.8±19.09µs     6.2 MB/sec    1.00   467.2±13.17µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.17ms     3.9 MB/sec    1.01      6.7±0.21ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.26ms     5.3 MB/sec    1.01      7.7±0.20ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1612.7±41.65µs    10.3 MB/sec    1.02  1652.0±45.83µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.9±9.47µs    14.8 MB/sec    1.02   202.6±10.45µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.11ms     7.5 MB/sec    1.05      3.6±0.21ms     7.1 MB/sec
parser/large/dataset.py                    1.01      6.2±0.24ms     6.6 MB/sec    1.00      6.1±0.19ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1193.9±36.69µs    13.9 MB/sec    1.04  1238.5±65.67µs    13.4 MB/sec
parser/numpy/globals.py                    1.00    118.3±4.36µs    24.9 MB/sec    1.00    118.1±4.73µs    25.0 MB/sec
parser/pydantic/types.py                   1.01      2.6±0.08ms     9.7 MB/sec    1.00      2.6±0.08ms     9.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.13ms     2.5 MB/sec    1.02     16.8±0.28ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.03      4.2±0.17ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    482.2±5.83µs     6.1 MB/sec    1.01    484.9±7.22µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.12ms     3.7 MB/sec    1.02      7.1±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.08ms     4.9 MB/sec    1.03      8.6±0.07ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1767.7±17.95µs     9.4 MB/sec    1.02  1809.4±21.19µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.9±2.86µs    14.8 MB/sec    1.03    206.7±8.40µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.8 MB/sec    1.03      3.9±0.04ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.7±0.05ms     6.1 MB/sec    1.02      6.9±0.05ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1282.3±19.71µs    13.0 MB/sec    1.02  1309.3±22.21µs    12.7 MB/sec
parser/numpy/globals.py                    1.00    130.7±1.63µs    22.6 MB/sec    1.02    132.6±3.15µs    22.2 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.03ms     8.9 MB/sec    1.01      2.9±0.03ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
