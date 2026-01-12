```yaml
number: 6084
title: "Remove relative import handling from `BindingKind::Import` case"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rel
created_at: 2023-07-26T04:06:57Z
updated_at: 2023-07-26T04:34:38Z
url: https://github.com/astral-sh/ruff/pull/6084
synced_at: 2026-01-12T15:55:20Z
```

# Remove relative import handling from `BindingKind::Import` case

---

_@charliermarsh_

## Summary

Only `ImportFrom` imports can be relative, this is just unused.


---

_Comment by @github-actions[bot] on 2023-07-26 04:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.03ms     4.4 MB/sec    1.00      9.3±0.04ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1863.2±2.41µs     8.9 MB/sec    1.00   1868.1±8.15µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    207.8±0.36µs    14.2 MB/sec    1.00    207.5±0.66µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.00ms     6.4 MB/sec    1.01      4.0±0.03ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.03ms     3.2 MB/sec    1.00     12.5±0.02ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.01ms     5.2 MB/sec    1.00      3.2±0.00ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.6±0.63µs     6.9 MB/sec    1.00    423.8±0.32µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.02ms     6.2 MB/sec    1.00      6.6±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1415.1±3.37µs    11.8 MB/sec    1.00   1407.7±1.21µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.7±0.85µs    19.0 MB/sec    1.00    155.7±0.56µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.00ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05     11.8±1.40ms     3.5 MB/sec    1.00     11.2±0.18ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.05ms     7.6 MB/sec    1.01      2.2±0.05ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    244.2±6.43µs    12.1 MB/sec    1.02   249.1±13.97µs    11.8 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.06ms     5.3 MB/sec    1.02      4.9±0.13ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.8±0.23ms     2.6 MB/sec    1.00     15.6±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.11ms     4.0 MB/sec    1.00      4.1±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   488.9±10.18µs     6.0 MB/sec    1.00   486.2±11.85µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.10ms     3.5 MB/sec    1.01      7.3±0.21ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.13ms     4.9 MB/sec    1.00      8.2±0.20ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1729.8±27.67µs     9.6 MB/sec    1.00  1736.5±33.34µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.02   203.2±15.50µs    14.5 MB/sec    1.00    198.4±8.52µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.07ms     6.8 MB/sec    1.00      3.7±0.07ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-26 04:17_

---

_Closed by @charliermarsh on 2023-07-26 04:17_

---

_Branch deleted on 2023-07-26 04:17_

---
