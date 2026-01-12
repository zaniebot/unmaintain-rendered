```yaml
number: 5538
title: Avoid syntax errors when rewriting str(dict) in f-strings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/curly
created_at: 2023-07-05T19:13:49Z
updated_at: 2023-07-05T19:45:06Z
url: https://github.com/astral-sh/ruff/pull/5538
synced_at: 2026-01-12T03:36:55Z
```

# Avoid syntax errors when rewriting str(dict) in f-strings

---

_Pull request opened by @charliermarsh on 2023-07-05 19:13_

Closes https://github.com/astral-sh/ruff/issues/5530.

---

_Label `bug` added by @charliermarsh on 2023-07-05 19:19_

---

_Merged by @charliermarsh on 2023-07-05 19:22_

---

_Closed by @charliermarsh on 2023-07-05 19:22_

---

_Branch deleted on 2023-07-05 19:22_

---

_Comment by @github-actions[bot] on 2023-07-05 19:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.03ms     5.2 MB/sec    1.00      7.8±0.03ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1745.6±2.58µs     9.5 MB/sec    1.00   1745.2±4.94µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    195.8±0.81µs    15.1 MB/sec    1.00    195.7±1.01µs    15.1 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.7 MB/sec    1.00      3.7±0.02ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.01     13.6±0.09ms     3.0 MB/sec    1.00     13.6±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    432.9±0.96µs     6.8 MB/sec    1.00    432.7±0.65µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.00      6.0±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.03ms     6.1 MB/sec    1.00      6.7±0.04ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1467.7±2.33µs    11.3 MB/sec    1.00   1467.8±2.30µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.2±0.24µs    17.6 MB/sec    1.00    166.6±0.83µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      9.7±0.11ms     4.2 MB/sec    1.00      9.2±0.08ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.1±0.03ms     8.0 MB/sec    1.00      2.0±0.03ms     8.3 MB/sec
formatter/numpy/globals.py                 1.02    234.9±5.17µs    12.6 MB/sec    1.00    229.9±7.71µs    12.8 MB/sec
formatter/pydantic/types.py                1.04      4.6±0.05ms     5.6 MB/sec    1.00      4.4±0.14ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.01     15.8±0.21ms     2.6 MB/sec    1.00     15.6±0.14ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.2±0.06ms     4.0 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    507.8±6.33µs     5.8 MB/sec    1.00    503.2±6.30µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.08ms     3.6 MB/sec    1.00      6.9±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.07ms     5.0 MB/sec    1.00      8.0±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1730.9±19.09µs     9.6 MB/sec    1.00  1707.8±15.93µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.02    202.9±3.53µs    14.5 MB/sec    1.00    198.7±3.91µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.0 MB/sec    1.00      3.6±0.05ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
