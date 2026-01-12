```yaml
number: 5852
title: Move unused deletion tracking to deferred analysis
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bind
created_at: 2023-07-18T02:10:50Z
updated_at: 2023-07-19T00:57:03Z
url: https://github.com/astral-sh/ruff/pull/5852
synced_at: 2026-01-12T03:30:22Z
```

# Move unused deletion tracking to deferred analysis

---

_Pull request opened by @charliermarsh on 2023-07-18 02:10_

## Summary

This PR moves the "unused exception" rule out of the visitor and into a deferred check. When we can base rules solely on the semantic model, we probably should, as it greatly simplifies the `Checker` itself.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-18 02:10_

---

_@charliermarsh reviewed on 2023-07-18 02:11_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4782 on 2023-07-18 02:11_

There should be a few others that we can move to this phase.

---

_Comment by @github-actions[bot] on 2023-07-18 03:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.02ms     4.4 MB/sec    1.02      9.4±0.02ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1851.1±6.41µs     9.0 MB/sec    1.01   1870.2±3.35µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    208.3±0.27µs    14.2 MB/sec    1.01    209.8±0.98µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.01      4.0±0.00ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.8±0.03ms     3.2 MB/sec    1.01     12.9±0.05ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.00ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.3±2.59µs     6.8 MB/sec    1.00    433.2±0.34µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.02ms     6.2 MB/sec    1.00      6.5±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1394.2±1.78µs    11.9 MB/sec    1.00   1397.7±1.42µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    153.3±0.29µs    19.2 MB/sec    1.00    153.8±0.27µs    19.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.7 MB/sec    1.00      3.0±0.00ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.2±0.18ms     3.6 MB/sec    1.00     11.0±0.09ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.03ms     7.8 MB/sec    1.00      2.1±0.02ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00    232.4±3.59µs    12.7 MB/sec    1.00    232.4±5.06µs    12.7 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.06ms     5.4 MB/sec    1.00      4.7±0.04ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.11ms     2.6 MB/sec    1.01     15.5±0.15ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.01      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   424.8±10.45µs     6.9 MB/sec    1.00    419.8±8.62µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.6 MB/sec    1.00      7.0±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.06ms     5.0 MB/sec    1.01      8.1±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1631.6±19.56µs    10.2 MB/sec    1.01  1642.5±17.00µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.3±5.21µs    17.1 MB/sec    1.00    172.6±1.97µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.0 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:4782 on 2023-07-18 06:21_

We need to be careful with the term *phase* to avoid confusion. If `check_bindings` is a phase, then `check_deferred_scoipes` and others are too. But what I understand is that these are not considered phases (at least, we only documented 4). 

---

_@MichaReiser approved on 2023-07-18 06:21_

---

_Merged by @charliermarsh on 2023-07-19 00:43_

---

_Closed by @charliermarsh on 2023-07-19 00:43_

---

_Branch deleted on 2023-07-19 00:43_

---
