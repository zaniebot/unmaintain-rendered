```yaml
number: 5162
title: Add Applicability to pyupgrade
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: applicability_pyupgrade
created_at: 2023-06-17T15:53:37Z
updated_at: 2023-06-17T21:59:29Z
url: https://github.com/astral-sh/ruff/pull/5162
synced_at: 2026-01-12T15:55:17Z
```

# Add Applicability to pyupgrade

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes some of #4184. The `manual` fixes here are because they involve deprecations and I'm hesitant to classify version-based fixes as automatic/suggested if they cause a regression in the user's code.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-17 16:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.3±0.07ms     5.5 MB/sec    1.01      7.4±0.11ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1546.2±20.46µs    10.8 MB/sec    1.00  1539.6±19.10µs    10.8 MB/sec
formatter/numpy/globals.py                 1.00    146.1±1.91µs    20.2 MB/sec    1.01    146.9±2.18µs    20.1 MB/sec
formatter/pydantic/types.py                1.00      3.0±0.04ms     8.5 MB/sec    1.01      3.0±0.04ms     8.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.14ms     2.6 MB/sec    1.01     15.5±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.04ms     4.3 MB/sec    1.00      3.9±0.05ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    487.4±6.93µs     6.1 MB/sec    1.00    485.1±6.63µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.06ms     3.8 MB/sec    1.00      6.7±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.07ms     5.2 MB/sec    1.00      7.8±0.06ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1714.2±19.46µs     9.7 MB/sec    1.00  1711.5±18.83µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.6±3.26µs    15.3 MB/sec    1.00    193.3±2.87µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.04ms     7.1 MB/sec    1.00      3.5±0.03ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.7±0.11ms     5.3 MB/sec    1.00      7.7±0.07ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1586.0±18.72µs    10.5 MB/sec    1.00  1591.0±20.03µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    153.4±3.51µs    19.2 MB/sec    1.00    153.8±4.13µs    19.2 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.03ms     8.0 MB/sec    1.00      3.2±0.05ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.15ms     2.5 MB/sec    1.01     16.2±0.19ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.0 MB/sec    1.01      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    492.4±6.92µs     6.0 MB/sec    1.01    497.7±9.11µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.01      7.0±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.07ms     4.9 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1743.9±16.92µs     9.5 MB/sec    1.01  1753.7±15.95µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    200.7±8.13µs    14.7 MB/sec    1.00    198.5±3.16µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.8 MB/sec    1.00      3.7±0.03ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-17 16:06_

I'd vote to make all of the "manual" fixes here "suggested". Users will need to opt-in to fixing them anyway. I think we should reserve "manual" for cases in which the fix is likely to be incorrect as-is.

---

_Comment by @evanrittenhouse on 2023-06-17 16:09_

My only worry there is that the user may not know that it's version-gated. I believe we have a way to check the Python version a user is running and emit diagnostics that way - should I add those checks to the rules in question here and change to suggested? @charliermarsh 

---

_Comment by @charliermarsh on 2023-06-17 16:11_

I believe all of those rules are properly gated already though (or they apply to all versions >= Python 3.7).

---

_Comment by @evanrittenhouse on 2023-06-17 16:23_

Ahhh gotcha, I didn't see any gates so I wanted to be safe. I'll change these back. 

Out of curiosity, where does that gating occur?

---

_Comment by @charliermarsh on 2023-06-17 16:39_

It depends on the rule. Some of these are applicable to all supported versions (>= 3.7), so we don't need to gate at all. Other pyupgrade rules are gated in `checkers/ast/mod.rs` (so they're just not invoked on unsupported versions). The deprecated import rule takes the `version` as a field on `ImportReplacer` and bases the checks on that.

---

_Comment by @evanrittenhouse on 2023-06-17 19:06_

Done - thanks for the suggestion @charliermarsh !

---

_Renamed from "Applicability pyupgrade" to "Add Applicability to pyupgrade rules" by @evanrittenhouse on 2023-06-17 19:06_

---

_Renamed from "Add Applicability to pyupgrade rules" to "Add Applicability to pyupgrade" by @evanrittenhouse on 2023-06-17 19:06_

---

_Comment by @charliermarsh on 2023-06-17 19:13_

(Looks like there's a small merge conflict.)

---

_Merged by @charliermarsh on 2023-06-17 19:33_

---

_Closed by @charliermarsh on 2023-06-17 19:33_

---

_Branch deleted on 2023-06-17 21:59_

---
