```yaml
number: 3891
title: "Ignore `PLW2901` when using typing cast"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/ignore-typing-cast
created_at: 2023-04-05T18:53:45Z
updated_at: 2023-04-06T01:38:27Z
url: https://github.com/astral-sh/ruff/pull/3891
synced_at: 2026-01-12T15:55:14Z
```

# Ignore `PLW2901` when using typing cast

---

_@dhruvmanila_

fixes: #3888
fixes: #3652

Currently, only single-target assignments are supported for `typing.cast`:
```python
x = cast(int, x)
# and not
x, y = cast(int, x), cast(str, y)
```

Also, annotated assignment isn't supported either as `mypy` thinks that's a reassignment due to it being type annotated:
```python
x = 1
x: int = cast(int, x)
# mypy: Name "x" already defined on line 1
```

---

_Comment by @github-actions[bot] on 2023-04-05 19:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.5±0.20ms     2.8 MB/sec    1.00     14.3±0.22ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.03ms     4.6 MB/sec    1.00      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    457.4±1.04µs     6.5 MB/sec    1.02    465.1±1.93µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.07ms     4.1 MB/sec    1.00      6.1±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.04ms     5.7 MB/sec    1.01      7.3±0.05ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1619.7±2.56µs    10.3 MB/sec    1.02   1654.4±3.62µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.0±0.46µs    16.5 MB/sec    1.04    185.5±0.54µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.01      3.4±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.07     21.9±0.77ms  1902.3 KB/sec    1.00     20.5±0.75ms  2028.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.12      5.9±0.29ms     2.8 MB/sec    1.00      5.2±0.26ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.10   685.4±49.49µs     4.3 MB/sec    1.00   625.1±30.42µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.14      9.8±0.60ms     2.6 MB/sec    1.00      8.6±0.40ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.09     11.2±0.41ms     3.6 MB/sec    1.00     10.3±0.36ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.3±0.09ms     7.1 MB/sec    1.00      2.3±0.09ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.01   259.2±25.18µs    11.4 MB/sec    1.00   256.1±18.34µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.24ms     5.2 MB/sec    1.00      4.9±0.23ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-05 22:02_

---

_Closed by @charliermarsh on 2023-04-05 22:02_

---

_Comment by @charliermarsh on 2023-04-05 22:02_

Really nice work.

---

_Branch deleted on 2023-04-06 01:23_

---

_Comment by @dhruvmanila on 2023-04-06 01:38_

> Really nice work.

Thank you! :)

---
