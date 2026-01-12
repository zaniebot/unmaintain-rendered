```yaml
number: 6126
title: Avoid walking past root when resolving imports
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: root
created_at: 2023-07-27T13:27:28Z
updated_at: 2023-07-27T13:54:27Z
url: https://github.com/astral-sh/ruff/pull/6126
synced_at: 2026-01-12T15:55:20Z
```

# Avoid walking past root when resolving imports

---

_@charliermarsh_

## Summary

Noticed in #5954: we walk _past_ the root rather than stopping _at_ the root when attempting to traverse along the parent path. It's effectively an off-by-one bug.

---

_Label `bug` added by @charliermarsh on 2023-07-27 13:30_

---

_Merged by @charliermarsh on 2023-07-27 13:38_

---

_@MichaReiser approved on 2023-07-27 13:38_

---

_Closed by @charliermarsh on 2023-07-27 13:38_

---

_Branch deleted on 2023-07-27 13:38_

---

_Comment by @github-actions[bot] on 2023-07-27 13:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.6±0.21ms     4.7 MB/sec    1.00      8.5±0.16ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01  1690.0±48.03µs     9.9 MB/sec    1.00  1665.2±31.95µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    181.6±1.30µs    16.2 MB/sec    1.01    183.8±4.63µs    16.1 MB/sec
formatter/pydantic/types.py                1.02      3.7±0.15ms     6.9 MB/sec    1.00      3.6±0.10ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.00     11.9±0.04ms     3.4 MB/sec    1.00     12.0±0.02ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.01ms     5.5 MB/sec    1.00      3.0±0.00ms     5.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    399.7±1.76µs     7.4 MB/sec    1.00    400.0±0.39µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.03ms     4.7 MB/sec    1.04      5.6±0.09ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      5.9±0.02ms     6.9 MB/sec    1.00      5.9±0.01ms     6.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1237.8±1.31µs    13.5 MB/sec    1.00   1240.1±1.11µs    13.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    134.4±0.82µs    22.0 MB/sec    1.00    134.6±0.22µs    21.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.02ms     9.8 MB/sec    1.00      2.6±0.01ms     9.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.5±0.12ms     3.9 MB/sec    1.03     10.8±0.13ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1997.6±32.04µs     8.3 MB/sec    1.03      2.1±0.04ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    213.3±5.87µs    13.8 MB/sec    1.05   223.0±12.99µs    13.2 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.06ms     5.8 MB/sec    1.04      4.5±0.06ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.0±0.31ms     2.7 MB/sec    1.00     15.0±0.17ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.04ms     4.3 MB/sec    1.00      3.9±0.04ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.01    458.2±6.94µs     6.4 MB/sec    1.00   453.8±11.30µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.9±0.09ms     3.7 MB/sec    1.00      6.8±0.11ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.02      7.7±0.08ms     5.3 MB/sec    1.00      7.6±0.06ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1553.5±20.00µs    10.7 MB/sec    1.00  1535.0±20.18µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    169.2±3.31µs    17.4 MB/sec    1.00    167.2±3.11µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.7 MB/sec    1.00      3.3±0.03ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
