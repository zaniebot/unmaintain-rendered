```yaml
number: 6778
title: "Add `networkx` to conventional aliases"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: enh/6763
created_at: 2023-08-22T16:09:13Z
updated_at: 2023-08-22T16:49:05Z
url: https://github.com/astral-sh/ruff/pull/6778
synced_at: 2026-01-12T15:55:22Z
```

# Add `networkx` to conventional aliases

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/6763

---

_@charliermarsh approved on 2023-08-22 16:15_

---

_Comment by @github-actions[bot] on 2023-08-22 16:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.4±0.01ms    12.0 MB/sec    1.00      3.4±0.02ms    12.0 MB/sec
formatter/numpy/ctypeslib.py               1.00    709.3±2.45µs    23.5 MB/sec    1.00    710.8±1.85µs    23.4 MB/sec
formatter/numpy/globals.py                 1.00     74.3±0.40µs    39.7 MB/sec    1.01     74.8±0.35µs    39.4 MB/sec
formatter/pydantic/types.py                1.00  1384.0±12.08µs    18.4 MB/sec    1.01  1398.3±16.70µs    18.2 MB/sec
linter/all-rules/large/dataset.py          1.00     10.3±0.08ms     4.0 MB/sec    1.04     10.7±0.12ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     6.0 MB/sec    1.01      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    390.4±1.65µs     7.6 MB/sec    1.00    389.3±1.52µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.04ms     4.8 MB/sec    1.01      5.4±0.03ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.4 MB/sec    1.06      5.8±0.02ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1206.6±2.41µs    13.8 MB/sec    1.04   1249.1±4.56µs    13.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    139.0±0.95µs    21.2 MB/sec    1.02    141.7±0.95µs    20.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.3 MB/sec    1.04      2.6±0.01ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.8±0.43ms     8.4 MB/sec     1.01      4.9±0.41ms     8.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   962.4±73.00µs    17.3 MB/sec     1.05  1012.5±105.04µs    16.4 MB/sec
formatter/numpy/globals.py                 1.03    101.9±9.75µs    29.0 MB/sec     1.00     99.2±8.33µs    29.7 MB/sec
formatter/pydantic/types.py                1.00  1928.6±110.48µs    13.2 MB/sec    1.04  1997.3±121.32µs    12.8 MB/sec
linter/all-rules/large/dataset.py          1.00     17.5±0.91ms     2.3 MB/sec     1.00     17.5±0.92ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.7±0.32ms     3.6 MB/sec     1.00      4.6±0.29ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   574.8±39.85µs     5.1 MB/sec     1.00   576.3±43.76µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.53ms     2.9 MB/sec     1.01      9.0±0.53ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      9.4±0.55ms     4.3 MB/sec     1.04      9.8±0.80ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1959.7±115.17µs     8.5 MB/sec    1.05      2.1±0.11ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   228.0±17.05µs    12.9 MB/sec     1.01   231.1±14.11µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.28ms     6.1 MB/sec     1.02      4.3±0.22ms     6.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @zanieb on 2023-08-22 16:49_

---

_Closed by @zanieb on 2023-08-22 16:49_

---

_Branch deleted on 2023-08-22 16:49_

---
