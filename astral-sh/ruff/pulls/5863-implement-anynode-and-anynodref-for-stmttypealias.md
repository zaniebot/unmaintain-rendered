```yaml
number: 5863
title: "Implement `AnyNode` and `AnyNodRef` for `StmtTypeAlias`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/695-nodes
created_at: 2023-07-18T14:41:03Z
updated_at: 2023-07-18T15:44:57Z
url: https://github.com/astral-sh/ruff/pull/5863
synced_at: 2026-01-12T15:55:19Z
```

# Implement `AnyNode` and `AnyNodRef` for `StmtTypeAlias`

---

_@zanieb_

Part of https://github.com/astral-sh/ruff/issues/5062

---

_@konstin approved on 2023-07-18 14:44_

---

_Comment by @github-actions[bot] on 2023-07-18 14:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.03ms     4.2 MB/sec    1.00      9.8±0.03ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1889.4±10.93µs     8.8 MB/sec    1.00   1894.2±7.49µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    206.7±0.35µs    14.3 MB/sec    1.00    206.9±0.26µs    14.3 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.07ms     2.9 MB/sec    1.00     14.0±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.7 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    379.0±1.06µs     7.8 MB/sec    1.00    375.8±0.96µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.8 MB/sec    1.01      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1466.0±2.56µs    11.4 MB/sec    1.00   1445.5±1.84µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    151.1±0.45µs    19.5 MB/sec    1.01    152.0±0.29µs    19.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.1 MB/sec    1.00      3.2±0.00ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.1±0.12ms     3.7 MB/sec    1.00     11.0±0.11ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.6 MB/sec    1.00      2.2±0.04ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    249.6±6.02µs    11.8 MB/sec    1.01   253.3±13.39µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.07ms     5.4 MB/sec    1.02      4.8±0.09ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.22ms     2.6 MB/sec    1.01     15.9±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.1 MB/sec    1.01      4.1±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   498.9±16.02µs     5.9 MB/sec    1.00    499.3±7.70µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.13ms     3.5 MB/sec    1.00      7.2±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.08ms     5.0 MB/sec    1.00      8.2±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1697.7±21.76µs     9.8 MB/sec    1.00  1699.4±20.85µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.8±4.66µs    15.5 MB/sec    1.00    191.5±3.75µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.06ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-18 15:15_

---

_Merged by @zanieb on 2023-07-18 15:44_

---

_Closed by @zanieb on 2023-07-18 15:44_

---

_Branch deleted on 2023-07-18 15:44_

---
