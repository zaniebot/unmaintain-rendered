```yaml
number: 4377
title: Avoid underflow in expected-special-method-signature
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/unexpected
created_at: 2023-05-11T16:41:06Z
updated_at: 2023-05-11T17:16:35Z
url: https://github.com/astral-sh/ruff/pull/4377
synced_at: 2026-01-12T15:55:15Z
```

# Avoid underflow in expected-special-method-signature

---

_@charliermarsh_

## Summary

This method didn't respect positional-only arguments, so it was possible to underflow when subtracting the number of arguments defaults from the number of positional arguments.

Closes #4372.


---

_@MichaReiser approved on 2023-05-11 16:47_

---

_Merged by @charliermarsh on 2023-05-11 16:47_

---

_Closed by @charliermarsh on 2023-05-11 16:47_

---

_Branch deleted on 2023-05-11 16:47_

---

_Comment by @github-actions[bot] on 2023-05-11 16:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     16.6±0.59ms     2.4 MB/sec    1.00     16.3±0.58ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.2±0.16ms     4.0 MB/sec    1.00      4.0±0.15ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   512.7±19.44µs     5.8 MB/sec    1.00   509.9±19.14µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.26ms     3.6 MB/sec    1.01      7.1±0.24ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.2±0.26ms     5.0 MB/sec    1.00      8.0±0.27ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1742.7±60.25µs     9.6 MB/sec    1.00  1730.9±55.26µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.04   218.9±11.03µs    13.5 MB/sec    1.00   211.4±13.95µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.8±0.12ms     6.7 MB/sec    1.00      3.6±0.13ms     7.1 MB/sec
parser/large/dataset.py                    1.02      6.6±0.26ms     6.2 MB/sec    1.00      6.5±0.19ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.03  1299.5±46.18µs    12.8 MB/sec    1.00  1265.4±45.76µs    13.2 MB/sec
parser/numpy/globals.py                    1.02    127.8±6.41µs    23.1 MB/sec    1.00    125.7±5.59µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.11ms     9.1 MB/sec    1.01      2.8±0.10ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     22.1±0.38ms  1881.0 KB/sec    1.00     21.4±0.61ms  1949.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.6±0.17ms     3.0 MB/sec    1.00      5.4±0.19ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.07   663.0±28.63µs     4.5 MB/sec    1.00   617.3±18.35µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.1±0.20ms     2.8 MB/sec    1.00      8.9±0.24ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.6±0.28ms     3.8 MB/sec    1.01     10.7±0.40ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.07ms     7.6 MB/sec    1.03      2.2±0.06ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   252.8±11.45µs    11.7 MB/sec    1.11   280.3±19.14µs    10.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.15ms     5.7 MB/sec    1.09      4.9±0.11ms     5.3 MB/sec
parser/large/dataset.py                    1.00      8.5±0.19ms     4.8 MB/sec    1.05      8.9±0.26ms     4.5 MB/sec
parser/numpy/ctypeslib.py                  1.00  1697.5±40.32µs     9.8 MB/sec    1.03  1740.5±40.35µs     9.6 MB/sec
parser/numpy/globals.py                    1.05    177.8±4.82µs    16.6 MB/sec    1.00    168.6±9.26µs    17.5 MB/sec
parser/pydantic/types.py                   1.02      3.9±0.10ms     6.6 MB/sec    1.00      3.8±0.09ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
