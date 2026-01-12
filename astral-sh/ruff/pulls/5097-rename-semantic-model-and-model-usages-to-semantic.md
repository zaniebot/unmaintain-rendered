```yaml
number: 5097
title: "Rename `semantic_model` and `model` usages to `semantic`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rename-semantic
created_at: 2023-06-14T18:52:34Z
updated_at: 2023-06-14T19:21:35Z
url: https://github.com/astral-sh/ruff/pull/5097
synced_at: 2026-01-12T15:55:17Z
```

# Rename `semantic_model` and `model` usages to `semantic`

---

_@charliermarsh_

## Summary

As discussed in Discord, and similar to oxc, we're going to refer to this as `.semantic()` everywhere.

While I was auditing usages of `model: &SemanticModel`, I also changed as many function signatures as I could find to consistently take the model as the _last_ argument, rather than the first.


---

_Renamed from "Rename semantic_model and model usages to semantic" to "Rename `semantic_model` and `model` usages to `semantic`" by @charliermarsh on 2023-06-14 18:52_

---

_Merged by @charliermarsh on 2023-06-14 19:01_

---

_Closed by @charliermarsh on 2023-06-14 19:01_

---

_Branch deleted on 2023-06-14 19:01_

---

_Comment by @github-actions[bot] on 2023-06-14 19:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.4±0.03ms     6.4 MB/sec    1.00      6.3±0.07ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1308.9±11.50µs    12.7 MB/sec    1.01   1316.7±6.47µs    12.6 MB/sec
formatter/numpy/globals.py                 1.00    124.7±2.13µs    23.7 MB/sec    1.00    124.8±0.34µs    23.6 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms    10.0 MB/sec    1.01      2.6±0.01ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.01     13.7±0.05ms     3.0 MB/sec    1.00     13.6±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     4.9 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.2±0.58µs     7.0 MB/sec    1.00    422.2±0.83µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.0±0.01ms     4.3 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      6.7±0.01ms     6.1 MB/sec    1.00      6.5±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1461.3±2.13µs    11.4 MB/sec    1.00   1442.3±3.96µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    163.4±0.32µs    18.1 MB/sec    1.00    161.8±0.95µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.3 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.09ms     5.2 MB/sec    1.00      7.8±0.13ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1602.3±25.68µs    10.4 MB/sec    1.00  1600.1±29.95µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    153.3±3.26µs    19.2 MB/sec    1.01    155.4±7.15µs    19.0 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     8.0 MB/sec    1.01      3.2±0.09ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.01     17.0±0.22ms     2.4 MB/sec    1.00     16.9±0.24ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.10ms     3.9 MB/sec    1.00      4.3±0.05ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   505.5±12.97µs     5.8 MB/sec    1.00    504.9±9.92µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.13ms     3.5 MB/sec    1.00      7.2±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.06ms     4.8 MB/sec    1.00      8.4±0.08ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1791.4±20.86µs     9.3 MB/sec    1.00  1791.9±34.40µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.3±8.10µs    14.5 MB/sec    1.00    202.7±4.56µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.00      3.8±0.04ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
