```yaml
number: 5256
title: Restore existing bindings when unbinding caught exceptions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/unbound
created_at: 2023-06-21T15:29:25Z
updated_at: 2023-06-21T17:27:18Z
url: https://github.com/astral-sh/ruff/pull/5256
synced_at: 2026-01-12T03:43:30Z
```

# Restore existing bindings when unbinding caught exceptions

---

_Pull request opened by @charliermarsh on 2023-06-21 15:29_

## Summary

In the latest release, we made some improvements to the semantic model, but our modifications to exception-unbinding are causing some false-positives. For example:

```py
try:
    v = 3
except ImportError as v:
    print(v)
else:
    print(v)
```

In the latest release, we started unbinding `v` after the `except` handler. (We used to restore the existing binding, the `v = 3`, but this was quite complicated.) Because we don't have full branch analysis, we can't then know that `v` is still bound in the `else` branch.

The solution here modifies `resolve_read` to skip-lookup when hitting unbound exceptions. So when store the "unbind" for `except ImportError as v`, we save the binding that it shadowed `v = 3`, and skip to that.

Closes #5249.

Closes #5250.


---

_Label `bug` added by @charliermarsh on 2023-06-21 15:29_

---

_Comment by @charliermarsh on 2023-06-21 15:44_

I have one other idea here (change `resolve_read` to continue searching up the shadowing chain).

---

_Comment by @github-actions[bot] on 2023-06-21 15:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.0±0.02ms     5.8 MB/sec    1.01      7.1±0.01ms     5.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   1396.3±2.50µs    11.9 MB/sec    1.01   1411.5±2.69µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    134.2±0.25µs    22.0 MB/sec    1.01    135.2±0.19µs    21.8 MB/sec
formatter/pydantic/types.py                1.00      2.9±0.01ms     8.8 MB/sec    1.01      2.9±0.02ms     8.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.8±0.04ms     2.9 MB/sec    1.00     13.8±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.8 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    367.5±1.17µs     8.0 MB/sec    1.00    366.6±1.56µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.7 MB/sec    1.00      7.2±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1500.2±2.56µs    11.1 MB/sec    1.00   1491.1±2.64µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    159.4±0.23µs    18.5 MB/sec    1.00    158.3±0.23µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.01      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.09ms     5.2 MB/sec    1.01      7.9±0.09ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1590.7±19.23µs    10.5 MB/sec    1.02  1619.0±23.41µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    155.3±3.96µs    19.0 MB/sec    1.03    159.3±5.75µs    18.5 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     7.9 MB/sec    1.02      3.3±0.07ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.01     15.3±0.23ms     2.7 MB/sec    1.00     15.2±0.15ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.06ms     4.1 MB/sec    1.00      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.7±8.72µs     5.9 MB/sec    1.00    497.6±7.49µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.17ms     3.7 MB/sec    1.00      6.8±0.08ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.10ms     5.1 MB/sec    1.01      8.0±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1706.8±31.65µs     9.8 MB/sec    1.01  1720.9±19.26µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    196.3±4.58µs    15.0 MB/sec    1.00    195.4±4.02µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.01      3.6±0.05ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-21 16:53_

---

_Closed by @charliermarsh on 2023-06-21 16:53_

---

_Branch deleted on 2023-06-21 16:54_

---
