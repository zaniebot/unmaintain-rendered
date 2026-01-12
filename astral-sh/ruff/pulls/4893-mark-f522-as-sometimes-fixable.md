```yaml
number: 4893
title: mark f522 as sometimes fixable
type: pull_request
state: merged
author: addisoncrump
labels:
  - fixes
assignees: []
merged: true
base: main
head: sometimes-f522
created_at: 2023-06-06T06:18:22Z
updated_at: 2023-06-06T13:14:31Z
url: https://github.com/astral-sh/ruff/pull/4893
synced_at: 2026-01-12T03:43:29Z
```

# mark f522 as sometimes fixable

---

_Pull request opened by @addisoncrump on 2023-06-06 06:18_

Closes #4892.

---

_Comment by @github-actions[bot] on 2023-06-06 06:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.7±0.03ms     7.1 MB/sec    1.02      5.8±0.02ms     7.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1192.7±3.97µs    14.0 MB/sec    1.01   1207.6±2.75µs    13.8 MB/sec
formatter/numpy/globals.py                 1.00    137.9±0.17µs    21.4 MB/sec    1.01    139.0±0.15µs    21.2 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.04ms    10.0 MB/sec    1.01      2.6±0.02ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.03ms     3.0 MB/sec    1.00     13.7±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    411.3±0.72µs     7.2 MB/sec    1.01    413.4±5.53µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.08ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.02ms     6.0 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1463.7±2.95µs    11.4 MB/sec    1.00   1457.2±2.03µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    162.9±0.51µs    18.1 MB/sec    1.00    162.0±0.27µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.00ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      7.0±0.10ms     5.8 MB/sec    1.00      6.9±0.12ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.01  1418.5±25.69µs    11.7 MB/sec    1.00  1403.0±18.85µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    159.4±4.16µs    18.5 MB/sec    1.01   160.3±10.52µs    18.4 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.04ms     8.3 MB/sec    1.00      3.1±0.05ms     8.3 MB/sec
linter/all-rules/large/dataset.py          1.01     17.0±0.14ms     2.4 MB/sec    1.00     16.9±0.18ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.07ms     3.9 MB/sec    1.00      4.2±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.5±6.95µs     6.0 MB/sec    1.00   495.6±11.25µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.09ms     3.6 MB/sec    1.00      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.08ms     4.9 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1773.8±16.83µs     9.4 MB/sec    1.00  1762.1±24.87µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    198.1±3.64µs    14.9 MB/sec    1.00    196.9±3.33µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.00      3.8±0.05ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-06 13:14_

---

_Closed by @charliermarsh on 2023-06-06 13:14_

---

_Comment by @charliermarsh on 2023-06-06 13:14_

Thanks :)

---

_Label `autofix` added by @charliermarsh on 2023-06-06 13:14_

---
