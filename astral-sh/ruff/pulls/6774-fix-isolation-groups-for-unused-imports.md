```yaml
number: 6774
title: Fix isolation groups for unused imports
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/isolate
created_at: 2023-08-22T15:29:51Z
updated_at: 2023-08-22T16:12:44Z
url: https://github.com/astral-sh/ruff/pull/6774
synced_at: 2026-01-12T02:52:04Z
```

# Fix isolation groups for unused imports

---

_Pull request opened by @charliermarsh on 2023-08-22 15:29_

## Summary

The isolation group for unused imports was relying on `checker.semantic().current_statement()`, which isn't valid for that rule, since it runs over the _scope_, not the statement. Instead, we need to lookup the isolation group based on the `NodeId` of the statement.

Our tests didn't catch this, because we mostly have cases that look like this:

```python
if TYPE_CHECKING:
    import shelve
    import importlib
```

In this case, the two fixes to remove the two unused imports are considered overlapping (since we delete the _full_ line, and the two _full_ lines touch, and we consider exactly-adjacent fixes to be overlapping), and so they don't run in a single pass due to the non-overlapping-fixes requirement. That is: the isolation groups aren't required for this case. They are, however, required for cases like:

```python
if TYPE_CHECKING:
    import shelve

    import importlib
```

...where the fixes don't overlap.

Closes https://github.com/astral-sh/ruff/issues/6758.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-08-22 15:29_

---

_Comment by @github-actions[bot] on 2023-08-22 15:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.07ms    10.8 MB/sec    1.06      4.0±0.04ms    10.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   790.6±20.60µs    21.1 MB/sec    1.07    843.7±8.85µs    19.7 MB/sec
formatter/numpy/globals.py                 1.00     83.2±2.61µs    35.5 MB/sec    1.08     90.0±1.19µs    32.8 MB/sec
formatter/pydantic/types.py                1.00  1559.7±29.68µs    16.4 MB/sec    1.05  1642.3±16.37µs    15.5 MB/sec
linter/all-rules/large/dataset.py          1.00     11.5±0.17ms     3.5 MB/sec    1.02     11.7±0.14ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.07ms     5.3 MB/sec    1.02      3.2±0.04ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    463.1±4.70µs     6.4 MB/sec    1.00    462.1±6.15µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.2±0.10ms     4.1 MB/sec    1.00      6.1±0.08ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.07ms     6.5 MB/sec    1.00      6.3±0.12ms     6.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1371.4±42.69µs    12.1 MB/sec    1.02  1402.2±18.21µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.5±3.01µs    19.0 MB/sec    1.07    167.1±3.93µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.04ms     9.2 MB/sec    1.03      2.9±0.04ms     8.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.9±0.33ms     8.3 MB/sec    1.01      5.0±0.33ms     8.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   993.7±74.88µs    16.8 MB/sec    1.00   986.1±69.83µs    16.9 MB/sec
formatter/numpy/globals.py                 1.00     98.3±6.13µs    30.0 MB/sec    1.04   102.4±11.65µs    28.8 MB/sec
formatter/pydantic/types.py                1.00      2.0±0.14ms    12.7 MB/sec    1.00      2.0±0.13ms    12.7 MB/sec
linter/all-rules/large/dataset.py          1.00     19.0±0.71ms     2.1 MB/sec    1.00     19.0±0.60ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.1±0.22ms     3.2 MB/sec    1.00      5.1±0.24ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   629.3±39.42µs     4.7 MB/sec    1.01   637.8±35.68µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00     10.0±0.54ms     2.6 MB/sec    1.00      9.9±0.50ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.01     10.6±0.65ms     3.9 MB/sec    1.00     10.5±0.59ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.7 MB/sec    1.01      2.2±0.16ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   266.1±18.18µs    11.1 MB/sec    1.00   266.6±16.41µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.6±0.22ms     5.5 MB/sec    1.00      4.6±0.21ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-22 15:55_

---

_Closed by @charliermarsh on 2023-08-22 15:55_

---

_Branch deleted on 2023-08-22 15:55_

---
