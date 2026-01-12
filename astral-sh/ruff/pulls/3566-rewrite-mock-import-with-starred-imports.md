```yaml
number: 3566
title: Rewrite mock import with starred imports
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/mock-import
created_at: 2023-03-17T00:23:20Z
updated_at: 2023-03-17T00:54:31Z
url: https://github.com/astral-sh/ruff/pull/3566
synced_at: 2026-01-12T04:39:45Z
```

# Rewrite mock import with starred imports

---

_Pull request opened by @charliermarsh on 2023-03-17 00:23_

## Summary

This adds support for rewriting `from mock import *`, which previously panicked.

Closes #3544.

## Test Plan

Add an additional case; update fixtures.


---

_Label `bug` added by @charliermarsh on 2023-03-17 00:23_

---

_Comment by @github-actions[bot] on 2023-03-17 00:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.01      9.3±0.03ms     4.4 MB/sec    1.00      9.1±0.02ms     4.4 MB/sec
linter/numpy/ctypeslib.py    1.00      3.0±0.01ms   110.5 MB/sec    1.08      3.3±0.00ms   102.1 MB/sec
linter/numpy/globals.py      1.00  1610.8±66.21µs   110.7 MB/sec    1.06   1706.4±4.50µs   104.5 MB/sec
linter/pydantic/types.py     1.00      4.3±0.01ms     5.9 MB/sec    1.00      4.3±0.01ms     5.9 MB/sec
```

#### Windows
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.01      8.8±0.08ms     4.6 MB/sec    1.00      8.7±0.07ms     4.7 MB/sec
linter/numpy/ctypeslib.py    1.00      2.8±0.02ms   120.5 MB/sec    1.00      2.8±0.03ms   120.8 MB/sec
linter/numpy/globals.py      1.00  1452.6±31.34µs   122.7 MB/sec    1.01  1468.6±30.31µs   121.4 MB/sec
linter/pydantic/types.py     1.01      4.1±0.10ms     6.3 MB/sec    1.00      4.0±0.05ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-17 00:54_

---

_Closed by @charliermarsh on 2023-03-17 00:54_

---

_Branch deleted on 2023-03-17 00:54_

---
