```yaml
number: 5057
title: Treat exception binding as explicit deletion
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/try
created_at: 2023-06-13T17:30:34Z
updated_at: 2023-06-13T18:10:19Z
url: https://github.com/astral-sh/ruff/pull/5057
synced_at: 2026-01-12T03:43:30Z
```

# Treat exception binding as explicit deletion

---

_Pull request opened by @charliermarsh on 2023-06-13 17:30_

## Summary

This PR corrects a misunderstanding I had related to Python's handling of bound exceptions.

Previously, I thought this code ran without error:

```py
def f():
    x = 1

    try:
        1 / 0
    except Exception as x:
        pass

    print(x)
```

My understanding was that `except Exception as x` bound `x` within the `except` block, but then restored the `x = 1` binding after exiting the block.

In practice, however, this throws a `UnboundLocalError` error, because `x` becomes "unbound" after exiting the exception handler. It's similar to a `del` statement in this way.

This PR removes our behavior to "restore" the previous binding. This could lead to faulty analysis in conditional blocks due to our lack of control flow analysis, but those same problems already exist for `del` statements.


---

_Comment by @github-actions[bot] on 2023-06-13 17:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.02ms     6.0 MB/sec    1.01      6.9±0.02ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1396.8±3.08µs    11.9 MB/sec    1.00   1392.2±3.88µs    12.0 MB/sec
formatter/numpy/globals.py                 1.00    138.2±0.61µs    21.3 MB/sec    1.00    137.8±0.89µs    21.4 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.6±0.08ms     2.8 MB/sec    1.00     14.7±0.11ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    370.8±0.91µs     8.0 MB/sec    1.00    369.8±1.81µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.02ms     4.1 MB/sec    1.01      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.03ms     5.6 MB/sec    1.01      7.4±0.03ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1540.3±3.92µs    10.8 MB/sec    1.00   1532.8±4.48µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.6±0.58µs    17.8 MB/sec    1.00    165.1±0.56µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.00ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.2±0.45ms     4.4 MB/sec    1.00      9.1±0.36ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1757.0±67.99µs     9.5 MB/sec    1.00  1754.6±50.80µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    160.1±7.48µs    18.4 MB/sec    1.04   166.5±12.97µs    17.7 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.27ms     7.1 MB/sec    1.02      3.7±0.16ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.02     20.5±0.49ms  2027.4 KB/sec    1.00     20.1±0.88ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.17ms     3.3 MB/sec    1.06      5.3±0.27ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   549.1±20.45µs     5.4 MB/sec    1.04   573.7±32.22µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.41ms     3.0 MB/sec    1.03      8.6±0.35ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.02     10.3±0.53ms     4.0 MB/sec    1.00     10.0±0.46ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1995.9±65.77µs     8.3 MB/sec    1.01      2.0±0.10ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.01   219.4±11.30µs    13.4 MB/sec    1.00   218.0±12.57µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.16ms     5.9 MB/sec    1.01      4.3±0.17ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-13 17:45_

---

_Closed by @charliermarsh on 2023-06-13 17:45_

---

_Branch deleted on 2023-06-13 17:45_

---
