```yaml
number: 4942
title: Avoid attributing runtime references to module-level imports
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cast-local
created_at: 2023-06-07T21:48:32Z
updated_at: 2023-06-07T22:21:19Z
url: https://github.com/astral-sh/ruff/pull/4942
synced_at: 2026-01-12T03:43:29Z
```

# Avoid attributing runtime references to module-level imports

---

_Pull request opened by @charliermarsh on 2023-06-07 21:48_

## Summary

This PR fixes a subtle case in import-use attribution, illustrated here:

```py
from __future__ import annotations

from typing import TYPE_CHECKING, cast

if TYPE_CHECKING:
    from threading import Thread


def fn(thread: Thread):
    from threading import Thread  # Flagged as F401

    casted_thread: Thread = cast(Thread, thread)
    return casted_thread

print(fn(1))
```

When we do `cast(Thread, thread)`, we need `Thread` to be attributed to the import within `fn`. This is subtle, because similar to Pyright and other type checkers, the `Thread` on the _left-hand_ side of the assignment (`casted_thread: Thread`) is resolved to the _global_ import.

Closes #4939.

---

_Label `bug` added by @charliermarsh on 2023-06-07 21:48_

---

_Merged by @charliermarsh on 2023-06-07 21:56_

---

_Closed by @charliermarsh on 2023-06-07 21:56_

---

_Branch deleted on 2023-06-07 21:56_

---

_Comment by @github-actions[bot] on 2023-06-07 22:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.5±0.39ms     5.4 MB/sec    1.11      8.4±0.26ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1485.1±96.39µs    11.2 MB/sec    1.15  1708.2±201.76µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00   165.5±15.61µs    17.8 MB/sec    1.16   191.5±13.18µs    15.4 MB/sec
formatter/pydantic/types.py                1.00      3.3±0.15ms     7.7 MB/sec    1.11      3.7±0.15ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.01     19.0±0.57ms     2.1 MB/sec    1.00     18.8±0.61ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.14ms     3.8 MB/sec    1.00      4.4±0.16ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   551.3±19.39µs     5.4 MB/sec    1.00   549.5±19.59µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.8±0.25ms     3.3 MB/sec    1.00      7.7±0.19ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.18ms     4.6 MB/sec    1.00      8.9±0.20ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1897.9±46.73µs     8.8 MB/sec    1.00  1874.9±59.83µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    224.1±7.52µs    13.2 MB/sec    1.00   225.0±10.27µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.11ms     6.4 MB/sec    1.00      4.0±0.09ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05      8.9±0.23ms     4.6 MB/sec    1.00      8.5±0.21ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1683.2±69.06µs     9.9 MB/sec    1.02  1719.8±50.22µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00    180.7±7.43µs    16.3 MB/sec    1.09   197.5±12.03µs    14.9 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.10ms     6.7 MB/sec    1.00      3.8±0.13ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.00     20.3±0.40ms     2.0 MB/sec    1.00     20.3±0.52ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.1±0.14ms     3.3 MB/sec    1.00      5.1±0.12ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   594.9±16.36µs     5.0 MB/sec    1.00   590.4±14.88µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.25ms     3.0 MB/sec    1.00      8.6±0.27ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.32ms     4.1 MB/sec    1.00      9.9±0.23ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     7.8 MB/sec    1.00      2.1±0.05ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    243.4±9.26µs    12.1 MB/sec    1.00    243.0±9.50µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.5±0.15ms     5.6 MB/sec    1.00      4.5±0.09ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
