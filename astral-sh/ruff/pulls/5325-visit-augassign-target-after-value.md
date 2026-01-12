```yaml
number: 5325
title: Visit AugAssign target after value
type: pull_request
state: merged
author: jgberry
labels: []
assignees: []
merged: true
base: main
head: visit-aug-assign-target-last
created_at: 2023-06-23T05:43:24Z
updated_at: 2023-06-23T20:57:59Z
url: https://github.com/astral-sh/ruff/pull/5325
synced_at: 2026-01-12T15:55:18Z
```

# Visit AugAssign target after value

---

_@jgberry_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

When visiting AugAssign in evaluation order, the AugAssign `target` should be visited after it's `value`. Based on my testing, the pseudo code for `a += b` is effectively:
```python
tmp = a
a = tmp.__iadd__(b)
```

That is, an ideal traversal order would look something like this:
1. load a
2. b
3. op
4. store a

But, there is only a single AST node which captures `a` in the statement `a += b`, so it cannot be traversed both before and after the traversal of `b` and the `op`.

Nonetheless, I think traversing `a` after `b` and the `op` makes the most sense for a number of reasons:
1. All the other assignment expressions traverse their `value`s before their `target`s. Having `AugAssign` traverse in the same order would be more consistent.
2. Within the AST, the `ctx` of the `target` for an `AugAssign` is `Store` (though technically this is a `Load` and `Store` operation, the AST only indicates it as a `Store`). Since the the store portion of the `AugAssign` occurs last, I think it makes sense to traverse the `target` last as well.

The effect of this is marginal, but it may have an impact on the behavior of #5271.

---

_Comment by @github-actions[bot] on 2023-06-23 05:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.0±0.02ms     5.8 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1476.7±1.48µs    11.3 MB/sec    1.00   1470.0±5.45µs    11.3 MB/sec
formatter/numpy/globals.py                 1.01    161.9±2.01µs    18.2 MB/sec    1.00    160.2±0.23µs    18.4 MB/sec
formatter/pydantic/types.py                1.02      3.5±0.00ms     7.3 MB/sec    1.00      3.4±0.01ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.04ms     3.0 MB/sec    1.07     14.7±0.04ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.04      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    362.2±1.32µs     8.1 MB/sec    1.03    371.6±3.16µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.06      6.4±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.8 MB/sec    1.15      8.2±0.01ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1476.6±3.19µs    11.3 MB/sec    1.12   1647.7±2.23µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.2±0.49µs    18.7 MB/sec    1.07    170.0±0.25µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.12      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.28     11.3±0.41ms     3.6 MB/sec    1.00      8.8±0.35ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.24      2.3±0.16ms     7.2 MB/sec    1.00  1877.0±67.37µs     8.9 MB/sec
formatter/numpy/globals.py                 1.09   265.0±12.20µs    11.1 MB/sec    1.00   242.1±31.14µs    12.2 MB/sec
formatter/pydantic/types.py                1.21      5.4±0.22ms     4.7 MB/sec    1.00      4.5±0.27ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.21     21.7±0.55ms  1917.8 KB/sec    1.00     17.9±0.99ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.20      5.7±0.17ms     2.9 MB/sec    1.00      4.7±0.26ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.25   699.2±32.13µs     4.2 MB/sec    1.00   559.3±31.80µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.29      9.9±0.51ms     2.6 MB/sec    1.00      7.7±0.42ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.23     11.2±0.37ms     3.6 MB/sec    1.00      9.1±0.48ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.22      2.3±0.07ms     7.1 MB/sec    1.00  1910.3±95.16µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.22   280.5±15.46µs    10.5 MB/sec    1.00   230.1±11.51µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.20      5.0±0.19ms     5.1 MB/sec    1.00      4.2±0.22ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-23 13:54_

---

_Closed by @charliermarsh on 2023-06-23 13:54_

---

_Comment by @charliermarsh on 2023-06-23 13:55_

Agree. I really appreciate your clear summaries.

---

_Branch deleted on 2023-06-23 20:57_

---
