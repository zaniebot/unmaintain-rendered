```yaml
number: 6296
title: "Use `range: _` in lieu of `range: _range`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/range_
created_at: 2023-08-03T01:38:29Z
updated_at: 2023-08-03T02:14:13Z
url: https://github.com/astral-sh/ruff/pull/6296
synced_at: 2026-01-12T15:55:21Z
```

# Use `range: _` in lieu of `range: _range`

---

_@charliermarsh_

## Summary

`range: _range` is slightly inconvenient because you can't use it multiple times within a single match, unlike `_`.


---

_Renamed from "Use range: _ in lieu of range: _range" to "Use `range: _` in lieu of `range: _range`" by @charliermarsh on 2023-08-03 01:38_

---

_Comment by @github-actions[bot] on 2023-08-03 01:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.5±0.06ms     4.8 MB/sec    1.00      8.5±0.08ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1681.9±42.02µs     9.9 MB/sec    1.00  1684.8±41.26µs     9.9 MB/sec
formatter/numpy/globals.py                 1.00    172.3±0.20µs    17.1 MB/sec    1.00    171.6±0.34µs    17.2 MB/sec
formatter/pydantic/types.py                1.00      3.7±0.08ms     7.0 MB/sec    1.00      3.7±0.07ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.00     11.3±0.17ms     3.6 MB/sec    1.00     11.3±0.14ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.03ms     5.7 MB/sec    1.00      2.9±0.04ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    317.3±2.66µs     9.3 MB/sec    1.00    317.8±0.82µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.03ms     5.0 MB/sec    1.00      5.1±0.03ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.02ms     6.8 MB/sec    1.00      6.0±0.02ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1199.7±10.95µs    13.9 MB/sec    1.00  1200.3±11.74µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    120.0±2.35µs    24.6 MB/sec    1.00    119.1±0.43µs    24.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.01ms     9.7 MB/sec    1.00      2.6±0.02ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.1±0.32ms     3.4 MB/sec    1.01     12.2±0.28ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.12ms     7.0 MB/sec    1.00      2.4±0.07ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00   257.8±12.87µs    11.4 MB/sec    1.02   262.9±12.72µs    11.2 MB/sec
formatter/pydantic/types.py                1.00      5.1±0.17ms     5.0 MB/sec    1.00      5.2±0.15ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.41ms     2.5 MB/sec    1.01     16.7±0.27ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.16ms     3.8 MB/sec    1.01      4.4±0.14ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   535.5±19.38µs     5.5 MB/sec    1.01   542.6±50.41µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.21ms     3.4 MB/sec    1.00      7.5±0.20ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.01      9.1±0.16ms     4.5 MB/sec    1.00      9.0±0.22ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1855.9±72.26µs     9.0 MB/sec    1.00  1841.3±45.53µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.05   216.7±24.33µs    13.6 MB/sec    1.00    206.9±7.36µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.2±0.50ms     6.1 MB/sec    1.00      4.0±0.11ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-03 02:11_

---

_Closed by @charliermarsh on 2023-08-03 02:11_

---

_Branch deleted on 2023-08-03 02:11_

---
