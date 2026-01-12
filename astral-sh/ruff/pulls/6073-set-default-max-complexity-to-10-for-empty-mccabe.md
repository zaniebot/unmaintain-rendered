```yaml
number: 6073
title: "Set default `max-complexity` to 10 for empty McCabe settings"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - configuration
assignees: []
merged: true
base: main
head: charlie/complex
created_at: 2023-07-25T15:27:41Z
updated_at: 2023-07-25T16:02:13Z
url: https://github.com/astral-sh/ruff/pull/6073
synced_at: 2026-01-12T03:30:22Z
```

# Set default `max-complexity` to 10 for empty McCabe settings

---

_Pull request opened by @charliermarsh on 2023-07-25 15:27_

Closes https://github.com/astral-sh/ruff/issues/6058.

---

_Renamed from "Set default max-complexity to 10 for empty McCabe settings" to "Set default `max-complexity` to 10 for empty McCabe settings" by @charliermarsh on 2023-07-25 15:27_

---

_Label `bug` added by @charliermarsh on 2023-07-25 15:27_

---

_Label `configuration` added by @charliermarsh on 2023-07-25 15:27_

---

_@zanieb approved on 2023-07-25 15:32_

---

_Merged by @charliermarsh on 2023-07-25 15:38_

---

_Closed by @charliermarsh on 2023-07-25 15:38_

---

_Branch deleted on 2023-07-25 15:38_

---

_Comment by @github-actions[bot] on 2023-07-25 15:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05     11.5±0.54ms     3.5 MB/sec    1.00     11.0±0.28ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.3±0.09ms     7.2 MB/sec    1.00      2.3±0.10ms     7.4 MB/sec
formatter/numpy/globals.py                 1.04   266.7±48.32µs    11.1 MB/sec    1.00   255.7±12.33µs    11.5 MB/sec
formatter/pydantic/types.py                1.01      5.0±0.17ms     5.1 MB/sec    1.00      4.9±0.26ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.61ms     2.6 MB/sec    1.00     15.7±0.49ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.11ms     4.2 MB/sec    1.00      4.0±0.13ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   541.3±25.11µs     5.5 MB/sec    1.00   539.3±20.96µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.30ms     3.5 MB/sec    1.00      7.2±0.18ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.29ms     5.0 MB/sec    1.00      8.2±0.15ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1749.9±65.04µs     9.5 MB/sec    1.01  1773.7±64.99µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    207.3±9.41µs    14.2 MB/sec    1.02    211.6±8.46µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.11ms     6.9 MB/sec    1.00      3.7±0.10ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.9±0.45ms     3.1 MB/sec    1.04     13.4±0.44ms     3.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.08ms     6.6 MB/sec    1.03      2.6±0.08ms     6.4 MB/sec
formatter/numpy/globals.py                 1.00   290.3±13.44µs    10.2 MB/sec    1.06   306.8±26.05µs     9.6 MB/sec
formatter/pydantic/types.py                1.00      5.5±0.20ms     4.6 MB/sec    1.05      5.8±0.20ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.01     18.0±0.52ms     2.3 MB/sec    1.00     17.9±0.36ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.16ms     3.5 MB/sec    1.00      4.7±0.14ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   573.3±22.57µs     5.1 MB/sec    1.00   575.0±24.76µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.3±0.30ms     3.1 MB/sec    1.00      8.2±0.23ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.6±0.21ms     4.2 MB/sec    1.00      9.6±0.29ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.0±0.06ms     8.3 MB/sec    1.00  1989.4±67.89µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    245.4±7.63µs    12.0 MB/sec    1.02   249.4±13.27µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.4±0.15ms     5.9 MB/sec    1.00      4.3±0.11ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
