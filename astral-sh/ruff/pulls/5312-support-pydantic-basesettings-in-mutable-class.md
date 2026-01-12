```yaml
number: 5312
title: "Support `pydantic.BaseSettings` in `mutable-class-default`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/base-settings
created_at: 2023-06-22T19:20:14Z
updated_at: 2023-06-22T19:44:59Z
url: https://github.com/astral-sh/ruff/pull/5312
synced_at: 2026-01-12T03:36:54Z
```

# Support `pydantic.BaseSettings` in `mutable-class-default`

---

_Pull request opened by @charliermarsh on 2023-06-22 19:20_

Closes #5308.

---

_Label `bug` added by @charliermarsh on 2023-06-22 19:20_

---

_Merged by @charliermarsh on 2023-06-22 19:27_

---

_Closed by @charliermarsh on 2023-06-22 19:27_

---

_Branch deleted on 2023-06-22 19:27_

---

_Comment by @github-actions[bot] on 2023-06-22 19:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.5±0.03ms     6.2 MB/sec    1.02      6.7±0.14ms     6.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1439.0±1.46µs    11.6 MB/sec    1.01   1451.0±5.52µs    11.5 MB/sec
formatter/numpy/globals.py                 1.00    161.7±0.30µs    18.2 MB/sec    1.00    162.1±0.23µs    18.2 MB/sec
formatter/pydantic/types.py                1.01      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.03     13.7±0.11ms     3.0 MB/sec    1.00     13.3±0.09ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.4±0.01ms     4.9 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.2±0.98µs     6.9 MB/sec    1.01    434.8±2.54µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.0±0.01ms     4.3 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.07      7.1±0.02ms     5.7 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05   1532.1±1.32µs    10.9 MB/sec    1.00   1458.4±7.10µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.04    168.2±0.74µs    17.5 MB/sec    1.00    162.3±0.85µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.2±0.01ms     8.0 MB/sec    1.00      3.0±0.00ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.20ms     4.3 MB/sec    1.00      9.4±0.24ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.10ms     8.0 MB/sec    1.00      2.0±0.07ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    227.1±9.08µs    13.0 MB/sec    1.01   230.2±14.63µs    12.8 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.21ms     5.3 MB/sec    1.00      4.8±0.17ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     18.6±0.40ms     2.2 MB/sec    1.01     18.8±0.42ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.16ms     3.4 MB/sec    1.02      5.0±0.37ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   600.1±19.73µs     4.9 MB/sec    1.01   607.5±24.25µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.22ms     3.1 MB/sec    1.01      8.4±0.27ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.19ms     4.2 MB/sec    1.00      9.7±0.21ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.05ms     8.0 MB/sec    1.02      2.1±0.09ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    241.4±6.62µs    12.2 MB/sec    1.02    245.5±7.76µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.12ms     5.8 MB/sec    1.01      4.4±0.12ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
