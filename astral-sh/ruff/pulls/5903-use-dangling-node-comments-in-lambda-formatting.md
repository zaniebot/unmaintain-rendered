```yaml
number: 5903
title: "Use `dangling_node_comments` in `lambda` formatting"
type: pull_request
state: merged
author: cnpryer
labels: []
assignees: []
merged: true
base: main
head: expr-lambda
created_at: 2023-07-20T01:47:50Z
updated_at: 2023-07-20T11:27:38Z
url: https://github.com/astral-sh/ruff/pull/5903
synced_at: 2026-01-12T03:30:22Z
```

# Use `dangling_node_comments` in `lambda` formatting

---

_Pull request opened by @cnpryer on 2023-07-20 01:47_

## Summary

Reading through more of the Python formatter code I noticed `dangling_node_comments`. Is this recommended over what was here originally? Seems nicer since it's the only `comments` usage. 

## Test Plan

Existing snapshots.


---

_Comment by @github-actions[bot] on 2023-07-20 01:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.9±0.02ms     4.1 MB/sec    1.00      9.8±0.02ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   1920.4±2.33µs     8.7 MB/sec    1.00   1903.4±7.26µs     8.7 MB/sec
formatter/numpy/globals.py                 1.01    208.7±1.25µs    14.1 MB/sec    1.00    206.6±0.45µs    14.3 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.01ms     6.0 MB/sec    1.00      4.2±0.02ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.04ms     3.0 MB/sec    1.01     13.7±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    377.4±1.12µs     7.8 MB/sec    1.00    375.9±0.56µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.02ms     4.1 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.03      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1450.0±3.87µs    11.5 MB/sec    1.01   1470.6±1.72µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.8±1.05µs    19.7 MB/sec    1.01    151.8±0.25µs    19.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.00ms     8.1 MB/sec    1.01      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     11.2±0.27ms     3.6 MB/sec    1.00     10.9±0.12ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.2±0.11ms     7.5 MB/sec    1.00      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.01    245.8±6.56µs    12.0 MB/sec    1.00    243.4±5.61µs    12.1 MB/sec
formatter/pydantic/types.py                1.03      4.8±0.13ms     5.3 MB/sec    1.00      4.7±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.15ms     2.6 MB/sec    1.00     15.4±0.21ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.08ms     4.1 MB/sec    1.00      4.0±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    493.7±8.66µs     6.0 MB/sec    1.00   492.8±19.96µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.1±0.31ms     3.6 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.1±0.09ms     5.0 MB/sec    1.00      8.0±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1675.7±19.64µs     9.9 MB/sec    1.00  1662.2±20.46µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    192.7±6.97µs    15.3 MB/sec    1.00    190.4±5.12µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.6±0.05ms     7.1 MB/sec    1.00      3.6±0.03ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-20 06:51_

Thank you! I like it.

> Is this recommended over what was here originally? 

My take on this:

Using `dangling_comments` is faster if the calling code contains conditional branching based on whether any comments exist because it avoids the comments lookup in `dangling_node_comments`. However, I do prefer `dangling_node_comments` if the calling code has no such conditional logic because it's simply less code (and the formatting code is often complicated enough that I happily take any simplification that I can get). 

---

_Merged by @MichaReiser on 2023-07-20 06:52_

---

_Closed by @MichaReiser on 2023-07-20 06:52_

---

_Branch deleted on 2023-07-20 11:27_

---
