```yaml
number: 4640
title: Remove deprecated --universal2 flag
type: pull_request
state: merged
author: charliermarsh
labels:
  - release
assignees: []
merged: true
base: main
head: charlie/universal
created_at: 2023-05-24T16:40:10Z
updated_at: 2023-05-24T17:31:11Z
url: https://github.com/astral-sh/ruff/pull/4640
synced_at: 2026-01-12T15:55:16Z
```

# Remove deprecated --universal2 flag

---

_@charliermarsh_

This was removed as part of the Maturin 1.0 release (https://github.com/PyO3/maturin/pull/1620).

---

_Review requested from @konstin by @charliermarsh on 2023-05-24 16:40_

---

_Label `release` added by @charliermarsh on 2023-05-24 16:41_

---

_@konstin approved on 2023-05-24 16:44_

sorry, i missed that we also have this configured

---

_Comment by @charliermarsh on 2023-05-24 16:45_

All good!

---

_Merged by @charliermarsh on 2023-05-24 17:00_

---

_Closed by @charliermarsh on 2023-05-24 17:00_

---

_Branch deleted on 2023-05-24 17:00_

---

_Comment by @github-actions[bot] on 2023-05-24 17:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.05ms     2.4 MB/sec    1.00     16.7±0.21ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.01ms     4.1 MB/sec    1.00      4.1±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.0±1.37µs     5.9 MB/sec    1.01    510.4±1.78µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.6 MB/sec    1.00      7.0±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.01ms     5.0 MB/sec    1.01      8.2±0.04ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1804.9±7.66µs     9.2 MB/sec    1.01   1815.5±4.20µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.5±0.43µs    14.5 MB/sec    1.02    208.0±5.68µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.00      3.7±0.01ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.2±0.05ms     6.5 MB/sec    1.00      6.2±0.02ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.01  1233.5±13.83µs    13.5 MB/sec    1.00   1224.7±1.08µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    125.9±0.56µs    23.4 MB/sec    1.00    125.6±1.10µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.01ms     9.5 MB/sec    1.00      2.7±0.00ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     23.0±0.99ms  1811.8 KB/sec    1.00     22.3±0.95ms  1869.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.7±0.26ms     2.9 MB/sec    1.00      5.5±0.27ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.03   670.3±38.32µs     4.4 MB/sec    1.00   651.6±33.85µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.6±0.44ms     2.7 MB/sec    1.00      9.6±0.46ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     11.2±0.43ms     3.6 MB/sec    1.01     11.3±0.49ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.12ms     7.3 MB/sec    1.05      2.4±0.10ms     7.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   275.5±15.22µs    10.7 MB/sec    1.05   289.8±16.71µs    10.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.23ms     5.2 MB/sec    1.05      5.2±0.26ms     4.9 MB/sec
parser/large/dataset.py                    1.00      8.7±0.48ms     4.7 MB/sec    1.01      8.8±0.40ms     4.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1632.8±72.05µs    10.2 MB/sec    1.01  1649.7±83.52µs    10.1 MB/sec
parser/numpy/globals.py                    1.00   167.2±15.88µs    17.7 MB/sec    1.01   169.0±11.05µs    17.5 MB/sec
parser/pydantic/types.py                   1.01      3.7±0.14ms     6.9 MB/sec    1.00      3.6±0.16ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
