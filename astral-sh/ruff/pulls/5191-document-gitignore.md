```yaml
number: 5191
title: Document gitignore
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: document_gitignore
created_at: 2023-06-19T18:51:58Z
updated_at: 2023-06-20T06:07:32Z
url: https://github.com/astral-sh/ruff/pull/5191
synced_at: 2026-01-12T15:55:17Z
```

# Document gitignore

---

_@konstin_

This docs-only change adds explanations to all custom .gitignore entries

---

_Comment by @github-actions[bot] on 2023-06-19 19:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.02ms     6.5 MB/sec    1.01      6.3±0.02ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1303.7±8.49µs    12.8 MB/sec    1.02   1330.3±8.12µs    12.5 MB/sec
formatter/numpy/globals.py                 1.00    125.1±1.32µs    23.6 MB/sec    1.03    128.4±0.41µs    23.0 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.9 MB/sec    1.02      2.6±0.03ms     9.8 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.05ms     3.1 MB/sec    1.02     13.3±0.05ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.00ms     5.1 MB/sec    1.00      3.2±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    407.7±0.39µs     7.2 MB/sec    1.01    410.3±0.59µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.6±0.01ms     4.5 MB/sec    1.01      5.7±0.01ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.02ms     6.2 MB/sec    1.03      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1418.0±2.58µs    11.7 MB/sec    1.01   1434.3±1.17µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.9±1.82µs    18.6 MB/sec    1.00    159.2±2.06µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.32ms     4.2 MB/sec    1.08     10.5±0.42ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1969.3±66.30µs     8.5 MB/sec    1.07      2.1±0.10ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00   196.1±14.63µs    15.0 MB/sec    1.06   207.2±15.17µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.16ms     6.5 MB/sec    1.11      4.3±0.16ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.05     21.1±0.60ms  1977.1 KB/sec    1.00     20.2±0.51ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.2±0.17ms     3.2 MB/sec    1.00      5.2±0.17ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.03   628.3±24.41µs     4.7 MB/sec    1.00   611.7±36.45µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      9.0±0.36ms     2.8 MB/sec    1.00      8.8±0.28ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.30ms     4.0 MB/sec    1.02     10.5±0.31ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.6 MB/sec    1.02      2.2±0.08ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.04   259.4±21.06µs    11.4 MB/sec    1.00   250.6±12.91µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.15ms     5.5 MB/sec    1.01      4.7±0.16ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-06-19 20:57_

Thank you :)

---

_Merged by @konstin on 2023-06-20 06:07_

---

_Closed by @konstin on 2023-06-20 06:07_

---

_Branch deleted on 2023-06-20 06:07_

---
