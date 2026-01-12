```yaml
number: 5277
title: Update docs to match updated logo and color palette
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/docs-refresh
created_at: 2023-06-22T00:48:45Z
updated_at: 2023-06-22T15:47:20Z
url: https://github.com/astral-sh/ruff/pull/5277
synced_at: 2026-01-12T15:55:18Z
```

# Update docs to match updated logo and color palette

---

_@charliermarsh_

<img width="1792" alt="Screen Shot 2023-06-21 at 8 48 24 PM" src="https://github.com/astral-sh/ruff/assets/1309177/efb19d3f-5e4a-455d-8dbd-5c8cebc01311">


## Test Plan

- `python scripts/generate_mkdocs.py`
- `mkdocs serve`

---

_@charliermarsh reviewed on 2023-06-22 00:49_

---

_Review comment by @charliermarsh on `mkdocs.template.yml`:24 on 2023-06-22 00:49_

We need to redo the dark color scheme before merging.

---

_Comment by @github-actions[bot] on 2023-06-22 01:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.9±0.01ms     5.9 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1425.7±5.36µs    11.7 MB/sec    1.00   1424.3±6.19µs    11.7 MB/sec
formatter/numpy/globals.py                 1.00    133.1±0.27µs    22.2 MB/sec    1.00    132.6±0.76µs    22.3 MB/sec
formatter/pydantic/types.py                1.01      2.9±0.01ms     8.9 MB/sec    1.00      2.9±0.00ms     8.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.02ms     3.0 MB/sec    1.00     13.7±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.8±2.15µs     8.2 MB/sec    1.02   369.2±10.57µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.8 MB/sec    1.00      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1480.8±2.09µs    11.2 MB/sec    1.00   1484.3±2.29µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    158.4±0.19µs    18.6 MB/sec    1.00    157.4±0.36µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.00ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.10ms     5.2 MB/sec    1.14      8.9±0.09ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1636.8±33.00µs    10.2 MB/sec    1.11  1815.9±25.69µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    156.9±3.04µs    18.8 MB/sec    1.11   174.1±11.98µs    17.0 MB/sec
formatter/pydantic/types.py                1.00      3.3±0.06ms     7.8 MB/sec    1.13      3.7±0.04ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.20ms     2.7 MB/sec    1.00     15.3±0.13ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.08ms     4.1 MB/sec    1.00      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    502.1±7.65µs     5.9 MB/sec    1.00    502.9±6.75µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.09ms     3.8 MB/sec    1.00      6.8±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.10ms     5.1 MB/sec    1.01      8.0±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1704.1±21.72µs     9.8 MB/sec    1.01  1715.4±30.90µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.7±4.32µs    14.9 MB/sec    1.01    198.8±3.96µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.06ms     7.1 MB/sec    1.01      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-22 15:47_

Superseded by #5283.

---

_Closed by @charliermarsh on 2023-06-22 15:47_

---
