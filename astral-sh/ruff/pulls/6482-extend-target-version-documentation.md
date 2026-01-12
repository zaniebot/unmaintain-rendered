```yaml
number: 6482
title: "Extend `target-version` documentation"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/target-version
created_at: 2023-08-10T16:31:58Z
updated_at: 2023-08-10T17:33:13Z
url: https://github.com/astral-sh/ruff/pull/6482
synced_at: 2026-01-12T02:52:04Z
```

# Extend `target-version` documentation

---

_Pull request opened by @zanieb on 2023-08-10 16:31_

Closes https://github.com/astral-sh/ruff/issues/6462


---

_Label `documentation` added by @zanieb on 2023-08-10 16:31_

---

_@konstin approved on 2023-08-10 16:46_

---

_Renamed from "Extend target-version documentaiton" to "Extend `target-version` documentation" by @zanieb on 2023-08-10 17:11_

---

_Merged by @zanieb on 2023-08-10 17:11_

---

_Closed by @zanieb on 2023-08-10 17:11_

---

_Branch deleted on 2023-08-10 17:11_

---

_Comment by @github-actions[bot] on 2023-08-10 17:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.9±0.30ms     3.7 MB/sec    1.00     10.7±0.34ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.1±0.06ms     7.8 MB/sec    1.00      2.1±0.07ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    236.9±7.56µs    12.5 MB/sec    1.01   238.5±11.69µs    12.4 MB/sec
formatter/pydantic/types.py                1.06      4.6±0.15ms     5.5 MB/sec    1.00      4.3±0.11ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.34ms     2.8 MB/sec    1.00     14.2±0.34ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.08ms     4.6 MB/sec    1.00      3.6±0.08ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   518.1±15.59µs     5.7 MB/sec    1.03   531.9±18.32µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.21ms     3.6 MB/sec    1.01      7.2±0.20ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      7.3±0.14ms     5.6 MB/sec    1.00      7.2±0.12ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1529.5±31.42µs    10.9 MB/sec    1.00  1498.3±44.91µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.02    184.5±7.26µs    16.0 MB/sec    1.00    180.6±5.49µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.13ms     8.2 MB/sec    1.02      3.2±0.08ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.2±0.24ms     3.3 MB/sec    1.00     12.2±0.33ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.4±0.15ms     7.1 MB/sec    1.00      2.3±0.08ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00   263.6±19.67µs    11.2 MB/sec    1.01   266.1±16.71µs    11.1 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.15ms     4.9 MB/sec    1.00      5.2±0.17ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.30ms     2.6 MB/sec    1.01     15.5±0.38ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.21ms     3.9 MB/sec    1.00      4.3±0.14ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   522.7±16.09µs     5.6 MB/sec    1.01   530.4±23.71µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.20ms     3.2 MB/sec    1.01      8.1±0.21ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.21ms     4.9 MB/sec    1.00      8.4±0.16ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1733.1±47.29µs     9.6 MB/sec    1.01  1750.3±47.39µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.2±7.71µs    14.8 MB/sec    1.01   201.3±19.49µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.09ms     6.8 MB/sec    1.00      3.7±0.09ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
