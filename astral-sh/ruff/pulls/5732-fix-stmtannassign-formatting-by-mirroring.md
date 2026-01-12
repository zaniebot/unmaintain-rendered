```yaml
number: 5732
title: "Fix `StmtAnnAssign` formatting by mirroring `StmtAssign`"
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: fix_ann_assign
created_at: 2023-07-13T10:02:13Z
updated_at: 2023-07-13T11:11:57Z
url: https://github.com/astral-sh/ruff/pull/5732
synced_at: 2026-01-12T03:30:21Z
```

# Fix `StmtAnnAssign` formatting by mirroring `StmtAssign`

---

_Pull request opened by @konstin on 2023-07-13 10:02_

## Summary

`StmtAnnAssign` would not insert parentheses when breaking the same way `StmtAssign` does, causing unstable formatting and likely some syntax errors.

## Test Plan

I added two regression tests, one for the value and one for the annotation.

---

_Review requested from @MichaReiser by @konstin on 2023-07-13 10:02_

---

_@konstin reviewed on 2023-07-13 10:02_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_ann_assign.rs`:38 on 2023-07-13 10:02_

I copied this from `StmtAssign`

---

_Label `formatter` added by @konstin on 2023-07-13 10:09_

---

_@MichaReiser approved on 2023-07-13 10:12_

---

_Comment by @github-actions[bot] on 2023-07-13 10:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.5±0.03ms     4.8 MB/sec    1.00      8.4±0.02ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1894.9±2.29µs     8.8 MB/sec    1.00   1891.7±7.72µs     8.8 MB/sec
formatter/numpy/globals.py                 1.01    206.6±0.69µs    14.3 MB/sec    1.00    205.5±0.35µs    14.4 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.01ms     6.0 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.08ms     2.9 MB/sec    1.01     14.4±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.02      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    367.6±1.20µs     8.0 MB/sec    1.00    369.1±1.09µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.1 MB/sec    1.01      6.3±0.03ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.7 MB/sec    1.02      7.3±0.03ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1462.3±12.67µs    11.4 MB/sec    1.02   1493.3±6.20µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.2±0.27µs    19.0 MB/sec    1.02    157.7±1.12µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.02      3.2±0.02ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.10     12.9±0.43ms     3.1 MB/sec    1.00     11.7±0.37ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.07      2.8±0.11ms     5.9 MB/sec    1.00      2.7±0.20ms     6.3 MB/sec
formatter/numpy/globals.py                 1.03   313.6±17.18µs     9.4 MB/sec    1.00   304.7±19.25µs     9.7 MB/sec
formatter/pydantic/types.py                1.06      6.2±0.27ms     4.1 MB/sec    1.00      5.8±0.30ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     19.4±0.39ms     2.1 MB/sec    1.01     19.6±0.47ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.18ms     3.3 MB/sec    1.00      5.1±0.18ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   616.9±24.32µs     4.8 MB/sec    1.00   612.4±26.83µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.40ms     2.9 MB/sec    1.00      8.8±0.31ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.26ms     4.1 MB/sec    1.01     10.0±0.46ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     7.9 MB/sec    1.00      2.1±0.07ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   244.2±10.83µs    12.1 MB/sec    1.02   247.9±12.71µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.16ms     5.8 MB/sec    1.02      4.5±0.16ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-07-13 10:26_

I've added the same fix for the annotation, which came up in another ecosystem example

---

_Merged by @konstin on 2023-07-13 10:51_

---

_Closed by @konstin on 2023-07-13 10:51_

---

_Branch deleted on 2023-07-13 10:51_

---
