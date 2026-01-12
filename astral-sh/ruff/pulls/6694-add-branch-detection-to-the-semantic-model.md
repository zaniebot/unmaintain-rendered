```yaml
number: 6694
title: Add branch detection to the semantic model
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/flow-node
created_at: 2023-08-19T19:15:56Z
updated_at: 2023-08-21T14:51:13Z
url: https://github.com/astral-sh/ruff/pull/6694
synced_at: 2026-01-12T02:52:04Z
```

# Add branch detection to the semantic model

---

_Pull request opened by @charliermarsh on 2023-08-19 19:15_

## Summary

We have a few rules that rely on detecting whether two statements are in different branches -- for example, different arms of an `if`-`else`. Historically, the way this was implemented is that, given two statement IDs, we'd find the common parent (by traversing upwards via our `Statements` abstraction); then identify branches "manually" by matching the parents against `try`, `if`, and `match`, and returning iterators over the arms; then check if there's an arm for which one of the statements is a child, and the other is not.

This has a few drawbacks:

1. First, the code is generally a bit hard to follow (Konsti mentioned this too when working on the `ElifElseClause` refactor). 

2. Second, this is the only place in the codebase where we need to go from `&Stmt` to `StatementID` -- _everywhere_ else, we only need to go in the _other_ direction. Supporting these lookups means we need to maintain a mapping from `&Stmt` to `StatementID` that includes every `&Stmt` in the program. (We _also_ end up maintaining a `depth` level for every statement.) I'd like to get rid of these requirements to improve efficiency, reduce complexity, and enable us to treat AST modes more generically in the future. (When I looked at adding the `&Expr` to our existing statement-tracking infrastructure, maintaining a hash map with all the statements noticeably hurt performance.)

The solution implemented here instead makes branches a first-class concept in the semantic model. Like with `Statements`, we now have a `Branches` abstraction, where each branch points to its optional parent. When we store statements, we store the `BranchID` alongside each statement. When we need to detect whether two statements are in the same branch, we just realize each statement's branch path and compare the two. (Assuming that the two statements are in the same scope, then they're on the same branch IFF one branch path is a subset of the other, starting from the top.) We then add some calls to the visitor to push and pop branches in the appropriate places, for `if`, `try`, and `match` statements.

Note that a branch is not 1:1 with a statement; instead, each branch is closer to a suite, but not _every_ suite is a branch. For example, each arm in an `if`-`elif`-`else` is a branch, but the `else` in a `for` loop is not considered a branch.

In addition to being much simpler, this should also be more efficient, since we've shed the entire `&Stmt` hash map, plus the `depth` that we track on `StatementWithParent` in favor of a single `Option<BranchID>` on `StatementWithParent` plus a single vector for all branches. The lookups should be faster too, since instead of doing a bunch of jumps around with the hash map + repeated recursive calls to find the common parents, we instead just do a few simple lookups in the `Branches` vector to realize and compare the branch paths.

## Test Plan

`cargo test` -- we have a lot of coverage for this, which we inherited from PyFlakes


---

_@charliermarsh reviewed on 2023-08-19 19:18_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/branch_detection.rs`:105 on 2023-08-19 19:18_

Glad to see this removed.

---

_@charliermarsh reviewed on 2023-08-19 19:19_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/statements.rs`:33 on 2023-08-19 19:19_

Glad to see this go.

---

_Label `internal` added by @charliermarsh on 2023-08-19 19:19_

---

_Comment by @github-actions[bot] on 2023-08-19 19:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.16ms    12.3 MB/sec    1.07      3.5±0.16ms    11.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   706.8±36.43µs    23.6 MB/sec    1.02   723.2±38.85µs    23.0 MB/sec
formatter/numpy/globals.py                 1.00     74.8±3.95µs    39.4 MB/sec    1.03     76.8±4.67µs    38.4 MB/sec
formatter/pydantic/types.py                1.00  1410.6±73.08µs    18.1 MB/sec    1.03  1448.3±67.45µs    17.6 MB/sec
linter/all-rules/large/dataset.py          1.00     10.6±0.32ms     3.8 MB/sec    1.00     10.7±0.34ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.09ms     5.8 MB/sec    1.02      2.9±0.11ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.02   420.4±17.81µs     7.0 MB/sec    1.00   412.0±16.52µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.6±0.20ms     4.5 MB/sec    1.00      5.5±0.21ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.01      5.7±0.20ms     7.2 MB/sec    1.00      5.6±0.15ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1258.5±44.41µs    13.2 MB/sec    1.00  1239.9±47.22µs    13.4 MB/sec
linter/default-rules/numpy/globals.py      1.06    153.2±7.70µs    19.3 MB/sec    1.00    145.1±6.17µs    20.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.12ms     9.9 MB/sec    1.00      2.6±0.09ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.05ms    11.0 MB/sec    1.00      3.7±0.05ms    10.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   762.6±17.12µs    21.8 MB/sec    1.00   762.2±11.70µs    21.8 MB/sec
formatter/numpy/globals.py                 1.00     78.9±1.49µs    37.4 MB/sec    1.01     79.3±1.52µs    37.2 MB/sec
formatter/pydantic/types.py                1.00  1527.6±24.91µs    16.7 MB/sec    1.01  1545.7±31.66µs    16.5 MB/sec
linter/all-rules/large/dataset.py          1.01     12.4±0.08ms     3.3 MB/sec    1.00     12.3±0.09ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.04ms     4.9 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.7±5.43µs     6.9 MB/sec    1.01    435.7±5.56µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.06ms     3.9 MB/sec    1.00      6.5±0.11ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.14      7.9±3.85ms     5.1 MB/sec    1.00      6.9±0.06ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1468.9±21.61µs    11.3 MB/sec    1.00  1473.2±17.68µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.2±3.15µs    17.1 MB/sec    1.00    172.9±2.20µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.2 MB/sec    1.00      3.1±0.04ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-19 20:53_

Small improvement on benchmarks (no decreases in performance, `large/dataset.py` improved by 1-2%):

```
linter/default-rules/numpy/globals.py
                        time:   [60.583 µs 60.774 µs 61.010 µs]
                        thrpt:  [48.363 MiB/s 48.552 MiB/s 48.705 MiB/s]
                 change:
                        time:   [-0.3999% -0.0452% +0.3162%] (p = 0.81 > 0.05)
                        thrpt:  [-0.3152% +0.0452% +0.4015%]
                        No change in performance detected.
Found 15 outliers among 100 measurements (15.00%)
  6 (6.00%) high mild
  9 (9.00%) high severe
Benchmarking linter/default-rules/pydantic/types.py: Collecting 100 samples in estimated 25.061 s (20k ite
linter/default-rules/pydantic/types.py
                        time:   [1.2391 ms 1.2398 ms 1.2406 ms]
                        thrpt:  [20.557 MiB/s 20.571 MiB/s 20.583 MiB/s]
                 change:
                        time:   [-1.3773% -1.1253% -0.8898%] (p = 0.00 < 0.05)
                        thrpt:  [+0.8978% +1.1381% +1.3966%]
                        Change within noise threshold.
Found 8 outliers among 100 measurements (8.00%)
  5 (5.00%) high mild
  3 (3.00%) high severe
Benchmarking linter/default-rules/numpy/ctypeslib.py: Collecting 100 samples in estimated 20.407 s (35k it
linter/default-rules/numpy/ctypeslib.py
                        time:   [578.64 µs 580.74 µs 583.33 µs]
                        thrpt:  [28.545 MiB/s 28.673 MiB/s 28.776 MiB/s]
                 change:
                        time:   [-1.0377% -0.7648% -0.4813%] (p = 0.00 < 0.05)
                        thrpt:  [+0.4836% +0.7707% +1.0486%]
                        Change within noise threshold.
Found 7 outliers among 100 measurements (7.00%)
  3 (3.00%) high mild
  4 (4.00%) high severe
Benchmarking linter/default-rules/large/dataset.py: Collecting 100 samples in estimated 59.321 s (20k iter
linter/default-rules/large/dataset.py
                        time:   [2.9103 ms 2.9139 ms 2.9192 ms]
                        thrpt:  [13.936 MiB/s 13.962 MiB/s 13.979 MiB/s]
                 change:
                        time:   [-2.1756% -2.0463% -1.9141%] (p = 0.00 < 0.05)
                        thrpt:  [+1.9514% +2.0890% +2.2239%]
                        Performance has improved.
Found 10 outliers among 100 measurements (10.00%)
  6 (6.00%) high mild
  4 (4.00%) high severe

Benchmarking linter/all-rules/numpy/globals.py: Collecting 100 samples in estimated 10.536 s (86k iteratio
linter/all-rules/numpy/globals.py
                        time:   [121.56 µs 121.70 µs 121.84 µs]
                        thrpt:  [24.217 MiB/s 24.246 MiB/s 24.273 MiB/s]
                 change:
                        time:   [-1.5244% -1.0506% -0.7164%] (p = 0.00 < 0.05)
                        thrpt:  [+0.7216% +1.0618% +1.5480%]
                        Change within noise threshold.
Found 3 outliers among 100 measurements (3.00%)
  1 (1.00%) high mild
  2 (2.00%) high severe
Benchmarking linter/all-rules/pydantic/types.py: Collecting 100 samples in estimated 25.274 s (10k iterati
linter/all-rules/pydantic/types.py
                        time:   [2.4946 ms 2.4955 ms 2.4965 ms]
                        thrpt:  [10.216 MiB/s 10.220 MiB/s 10.223 MiB/s]
                 change:
                        time:   [-1.2063% -0.9096% -0.6549%] (p = 0.00 < 0.05)
                        thrpt:  [+0.6592% +0.9179% +1.2210%]
                        Change within noise threshold.
Found 13 outliers among 100 measurements (13.00%)
  8 (8.00%) high mild
  5 (5.00%) high severe
Benchmarking linter/all-rules/numpy/ctypeslib.py: Collecting 100 samples in estimated 25.683 s (20k iterat
linter/all-rules/numpy/ctypeslib.py
                        time:   [1.2649 ms 1.2658 ms 1.2670 ms]
                        thrpt:  [13.143 MiB/s 13.155 MiB/s 13.164 MiB/s]
                 change:
                        time:   [+0.1974% +0.7212% +1.3268%] (p = 0.01 < 0.05)
                        thrpt:  [-1.3094% -0.7160% -0.1970%]
                        Change within noise threshold.
Found 13 outliers among 100 measurements (13.00%)
  4 (4.00%) high mild
  9 (9.00%) high severe
Benchmarking linter/all-rules/large/dataset.py: Collecting 100 samples in estimated 50.342 s (10k iteratio
linter/all-rules/large/dataset.py
                        time:   [4.9667 ms 4.9736 ms 4.9828 ms]
                        thrpt:  [8.1646 MiB/s 8.1797 MiB/s 8.1912 MiB/s]
                 change:
                        time:   [-1.7667% -1.5446% -1.3479%] (p = 0.00 < 0.05)
                        thrpt:  [+1.3663% +1.5688% +1.7985%]
                        Performance has improved.
Found 11 outliers among 100 measurements (11.00%)
  5 (5.00%) high mild
  6 (6.00%) high severe
```

---

_Comment by @charliermarsh on 2023-08-19 20:54_

(I don't think any of the benchmarks even trigger the branch detection though, that's just from not hashing the statement etc. presumedly.)

---

_Comment by @charliermarsh on 2023-08-19 21:19_

I replaced one of the benchmarks with a file that includes a lot of redefinitions-within-branches, small improvement:

```
linter/default-rules/numpy/globals.py
                        time:   [60.227 µs 60.337 µs 60.457 µs]
                        thrpt:  [48.806 MiB/s 48.903 MiB/s 48.993 MiB/s]
                 change:
                        time:   [-1.6113% -1.3619% -1.1125%] (p = 0.00 < 0.05)
                        thrpt:  [+1.1251% +1.3807% +1.6377%]
                        Performance has improved.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild

Benchmarking linter/all-rules/numpy/globals.py: Collecting 100 samples in estimated 10.464 s (86k iteratio
linter/all-rules/numpy/globals.py
                        time:   [121.33 µs 121.46 µs 121.60 µs]
                        thrpt:  [24.266 MiB/s 24.294 MiB/s 24.320 MiB/s]
                 change:
                        time:   [-1.6174% -1.0966% -0.6405%] (p = 0.00 < 0.05)
                        thrpt:  [+0.6446% +1.1088% +1.6440%]
                        Change within noise threshold.
Found 3 outliers among 100 measurements (3.00%)
  2 (2.00%) high mild
  1 (1.00%) high severe
```

---

_Comment by @charliermarsh on 2023-08-19 21:24_

Merging as a non-behavior-changing refactor that I have high confidence in and don't want to distract others by. If anyone is eager to review I'll happily address follow-up feedback.


---

_Merged by @charliermarsh on 2023-08-19 21:28_

---

_Closed by @charliermarsh on 2023-08-19 21:28_

---

_Branch deleted on 2023-08-19 21:28_

---

_Comment by @MichaReiser on 2023-08-21 07:01_

This new data structure has similarities to our data flow analysis. Do you see way how we could merge the two? 

Out of scope: It would be nice if we only built up these more advanced analyses if a rule consuming the data is enabled. 

---

_@konstin reviewed on 2023-08-21 14:51_

---

_Review comment by @konstin on `crates/ruff_python_semantic/src/branches.rs`:19 on 2023-08-21 14:51_

i don't like that the macro adds the field to the struct, it makes it hard to follow and hard to IDE/tool-inspect what data that struct actually is

---
