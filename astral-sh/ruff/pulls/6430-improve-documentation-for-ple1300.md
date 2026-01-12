```yaml
number: 6430
title: "Improve documentation for `PLE1300`"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/PLE1300
created_at: 2023-08-08T18:49:19Z
updated_at: 2023-08-08T20:40:27Z
url: https://github.com/astral-sh/ruff/pull/6430
synced_at: 2026-01-12T15:55:21Z
```

# Improve documentation for `PLE1300`

---

_@zanieb_

_No description provided._

---

_Label `documentation` added by @zanieb on 2023-08-08 18:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/bad_string_format_character.rs`:43 on 2023-08-08 18:51_

This should also be added on `pub(crate) fn percent` below.

---

_@charliermarsh reviewed on 2023-08-08 18:51_

---

_@charliermarsh approved on 2023-08-08 18:51_

---

_Renamed from "Improve documentatino for `PLE1300`" to "Improve documentation for `PLE1300`" by @charliermarsh on 2023-08-08 18:55_

---

_Comment by @github-actions[bot] on 2023-08-08 19:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.26     10.8±0.44ms     3.8 MB/sec    1.00      8.5±0.34ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.18      2.0±0.09ms     8.3 MB/sec    1.00  1706.7±108.02µs     9.8 MB/sec
formatter/numpy/globals.py                 1.10    224.7±8.46µs    13.1 MB/sec    1.00   203.7±20.88µs    14.5 MB/sec
formatter/pydantic/types.py                1.18      4.4±0.14ms     5.8 MB/sec    1.00      3.7±0.32ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.24     13.3±0.34ms     3.1 MB/sec    1.00     10.7±0.52ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      3.1±0.26ms     5.4 MB/sec    1.00      2.9±0.16ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.18   477.4±17.51µs     6.2 MB/sec    1.00   405.2±25.98µs     7.3 MB/sec
linter/all-rules/pydantic/types.py         1.24      6.1±0.21ms     4.2 MB/sec    1.00      4.9±0.27ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.08      6.8±0.20ms     6.0 MB/sec    1.00      6.3±0.46ms     6.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.17  1393.2±33.26µs    12.0 MB/sec    1.00  1195.7±62.25µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.14    159.0±5.11µs    18.6 MB/sec    1.00   139.5±10.56µs    21.1 MB/sec
linter/default-rules/pydantic/types.py     1.14      2.9±0.09ms     8.7 MB/sec    1.00      2.6±0.16ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     12.2±0.53ms     3.3 MB/sec    1.00     12.1±0.47ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.10ms     7.0 MB/sec    1.02      2.4±0.12ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   275.5±30.07µs    10.7 MB/sec    1.00   274.2±16.42µs    10.8 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.26ms     4.9 MB/sec    1.00      5.2±0.23ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.66ms     2.5 MB/sec    1.00     16.4±0.56ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.28ms     3.8 MB/sec    1.02      4.5±0.17ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.02   547.2±27.88µs     5.4 MB/sec    1.00   538.0±29.98µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.6±0.28ms     3.3 MB/sec    1.00      7.4±0.39ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      8.8±0.40ms     4.6 MB/sec    1.00      8.7±0.39ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1845.2±79.92µs     9.0 MB/sec    1.00  1820.3±73.19µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.01   223.5±14.57µs    13.2 MB/sec    1.00   222.2±13.86µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.28ms     6.4 MB/sec    1.00      4.0±0.19ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @zanieb on 2023-08-08 20:16_

---

_Closed by @zanieb on 2023-08-08 20:16_

---

_Branch deleted on 2023-08-08 20:16_

---
