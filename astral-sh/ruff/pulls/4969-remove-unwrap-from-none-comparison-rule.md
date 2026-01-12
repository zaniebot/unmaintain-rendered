```yaml
number: 4969
title: "Remove `unwrap` from none-comparison rule"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/wrap
created_at: 2023-06-08T18:03:49Z
updated_at: 2023-06-08T18:40:44Z
url: https://github.com/astral-sh/ruff/pull/4969
synced_at: 2026-01-12T03:43:29Z
```

# Remove `unwrap` from none-comparison rule

---

_Pull request opened by @charliermarsh on 2023-06-08 18:03_

_No description provided._

---

_@charliermarsh reviewed on 2023-06-08 18:04_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/literal_comparisons.rs`:158 on 2023-06-08 18:04_

This is all just indentation.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/literal_comparisons.rs`:194 on 2023-06-08 18:04_

(But we now match here, rather than matching on `Cmpop` and then calling `op.into()` to convert.

---

_@charliermarsh reviewed on 2023-06-08 18:04_

---

_Merged by @charliermarsh on 2023-06-08 18:21_

---

_Closed by @charliermarsh on 2023-06-08 18:21_

---

_Branch deleted on 2023-06-08 18:21_

---

_Comment by @github-actions[bot] on 2023-06-08 18:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.02ms     6.5 MB/sec    1.01      6.3±0.04ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1112.0±1.67µs    15.0 MB/sec    1.01   1123.0±3.45µs    14.8 MB/sec
formatter/numpy/globals.py                 1.00    125.4±0.26µs    23.5 MB/sec    1.00    125.6±0.21µs    23.5 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms    10.0 MB/sec    1.00      2.5±0.00ms    10.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.11ms     2.9 MB/sec    1.00     14.0±0.11ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.4±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.2±4.31µs     7.1 MB/sec    1.00    418.3±0.55µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.03ms     4.4 MB/sec    1.01      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.04ms     6.1 MB/sec    1.01      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1453.8±4.22µs    11.5 MB/sec    1.00   1455.9±4.94µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.4±1.74µs    18.4 MB/sec    1.00    160.8±0.48µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.5 MB/sec    1.01      3.0±0.00ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.10      8.4±0.09ms     4.8 MB/sec    1.00      7.7±0.09ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.09  1448.1±31.02µs    11.5 MB/sec    1.00  1325.8±18.88µs    12.6 MB/sec
formatter/numpy/globals.py                 1.05    151.8±3.48µs    19.4 MB/sec    1.00    144.0±4.61µs    20.5 MB/sec
formatter/pydantic/types.py                1.09      3.4±0.05ms     7.5 MB/sec    1.00      3.1±0.07ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.01     17.5±0.33ms     2.3 MB/sec    1.00     17.4±0.26ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.06ms     3.8 MB/sec    1.00      4.3±0.10ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   508.4±12.34µs     5.8 MB/sec    1.00    505.3±9.11µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.12ms     3.5 MB/sec    1.00      7.3±0.18ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.09ms     4.8 MB/sec    1.00      8.5±0.11ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1787.1±23.78µs     9.3 MB/sec    1.01  1803.3±23.63µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.6±5.13µs    14.5 MB/sec    1.00    203.9±3.36µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.00      3.8±0.04ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
