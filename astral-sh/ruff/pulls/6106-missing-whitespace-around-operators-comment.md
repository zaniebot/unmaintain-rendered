```yaml
number: 6106
title: missing-whitespace-around-operators comment
type: pull_request
state: merged
author: arembridge
labels:
  - documentation
assignees: []
merged: true
base: main
head: missing-whitespace-around-operators
created_at: 2023-07-26T21:10:22Z
updated_at: 2023-07-27T18:52:44Z
url: https://github.com/astral-sh/ruff/pull/6106
synced_at: 2026-01-12T15:55:20Z
```

# missing-whitespace-around-operators comment

---

_@arembridge_

**Summary**

Updated doc comments for `missing_whitespace_around_operator.rs`. Online docs also benefit from this update.

**Test Plan**

Checked docs via [mkdocs](https://github.com/astral-sh/ruff/blob/389fe13c934fe73679474006412b1eded4a2cad0/CONTRIBUTING.md?plain=1#L267-L296)

---

_Comment by @github-actions[bot] on 2023-07-26 21:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.1±0.05ms     4.5 MB/sec    1.00      9.0±0.03ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.01   1728.7±7.71µs     9.6 MB/sec    1.00   1714.9±3.43µs     9.7 MB/sec
formatter/numpy/globals.py                 1.01    180.1±1.87µs    16.4 MB/sec    1.00    178.7±0.46µs    16.5 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.02ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.07ms     3.2 MB/sec    1.01     12.7±0.07ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.01ms     5.2 MB/sec    1.01      3.2±0.01ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    340.2±0.99µs     8.7 MB/sec    1.02    347.9±1.63µs     8.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.6±0.05ms     4.5 MB/sec    1.01      5.7±0.01ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.02ms     6.4 MB/sec    1.02      6.5±0.01ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1265.3±5.20µs    13.2 MB/sec    1.02   1288.1±3.32µs    12.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    126.1±0.30µs    23.4 MB/sec    1.01    127.8±0.61µs    23.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.00ms     9.2 MB/sec    1.02      2.8±0.01ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     12.8±0.51ms     3.2 MB/sec    1.00     12.6±0.46ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.10ms     7.0 MB/sec    1.02      2.4±0.16ms     6.8 MB/sec
formatter/numpy/globals.py                 1.00   263.0±13.63µs    11.2 MB/sec    1.02   268.7±17.97µs    11.0 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.21ms     4.9 MB/sec    1.02      5.3±0.21ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.02     18.1±0.68ms     2.2 MB/sec    1.00     17.8±0.45ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.7±0.17ms     3.5 MB/sec    1.00      4.6±0.18ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   559.8±26.46µs     5.3 MB/sec    1.02   568.6±29.80µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.1±0.32ms     3.1 MB/sec    1.00      8.0±0.28ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.02      9.2±0.27ms     4.4 MB/sec    1.00      9.0±0.29ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1850.0±54.93µs     9.0 MB/sec    1.00  1855.6±57.66µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.2±8.82µs    14.5 MB/sec    1.00    203.8±9.62µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.0±0.12ms     6.4 MB/sec    1.00      4.0±0.15ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Missing whitespace around operators" to "missing-whitespace-around-operators" by @arembridge on 2023-07-26 22:26_

---

_Renamed from "missing-whitespace-around-operators" to "missing-whitespace-around-operators comment" by @arembridge on 2023-07-26 22:27_

---

_Label `documentation` added by @MichaReiser on 2023-07-27 05:56_

---

_Merged by @charliermarsh on 2023-07-27 18:52_

---

_Closed by @charliermarsh on 2023-07-27 18:52_

---
