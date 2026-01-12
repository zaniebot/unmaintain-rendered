```yaml
number: 4976
title: Support concatenated string key removals
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/kwargs
created_at: 2023-06-09T04:16:48Z
updated_at: 2023-06-09T05:00:46Z
url: https://github.com/astral-sh/ruff/pull/4976
synced_at: 2026-01-12T03:43:29Z
```

# Support concatenated string key removals

---

_Pull request opened by @charliermarsh on 2023-06-09 04:16_

Closes #4975.


---

_Label `bug` added by @charliermarsh on 2023-06-09 04:16_

---

_Comment by @addisoncrump on 2023-06-09 04:24_

Should we add a unit test for this as well? I think the minified example from #4975 should work.

---

_Comment by @charliermarsh on 2023-06-09 04:27_

Good call.

---

_Merged by @charliermarsh on 2023-06-09 04:56_

---

_Closed by @charliermarsh on 2023-06-09 04:56_

---

_Branch deleted on 2023-06-09 04:56_

---

_Comment by @github-actions[bot] on 2023-06-09 04:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.9±0.02ms     5.9 MB/sec    1.00      6.9±0.04ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.01   1221.8±4.55µs    13.6 MB/sec    1.00   1211.2±3.56µs    13.7 MB/sec
formatter/numpy/globals.py                 1.01    135.6±0.45µs    21.8 MB/sec    1.00    134.0±0.66µs    22.0 MB/sec
formatter/pydantic/types.py                1.02      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.8±0.40ms     2.8 MB/sec    1.01     14.9±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.02ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    369.4±1.37µs     8.0 MB/sec    1.00    369.5±1.44µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.00      6.3±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.02ms     5.5 MB/sec    1.01      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1534.3±5.74µs    10.9 MB/sec    1.00   1536.7±6.08µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.4±0.64µs    18.0 MB/sec    1.00    164.4±0.46µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.8 MB/sec    1.03      3.4±0.02ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.12ms     5.3 MB/sec    1.00      7.7±0.07ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1355.2±24.46µs    12.3 MB/sec    1.00  1349.4±28.96µs    12.3 MB/sec
formatter/numpy/globals.py                 1.00    147.2±3.17µs    20.0 MB/sec    1.02    150.2±6.12µs    19.6 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     8.1 MB/sec    1.00      3.1±0.05ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.02     17.3±0.21ms     2.4 MB/sec    1.00     16.9±0.19ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.04ms     3.9 MB/sec    1.00      4.3±0.09ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    504.7±8.07µs     5.8 MB/sec    1.00   500.7±11.24µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.12ms     3.5 MB/sec    1.00      7.2±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.06      8.8±0.06ms     4.6 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1843.7±23.25µs     9.0 MB/sec    1.00  1765.4±18.41µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    205.5±3.18µs    14.4 MB/sec    1.00    200.7±2.91µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.9±0.03ms     6.5 MB/sec    1.00      3.7±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
