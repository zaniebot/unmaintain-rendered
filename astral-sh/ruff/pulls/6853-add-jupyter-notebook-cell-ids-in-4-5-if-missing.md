```yaml
number: 6853
title: Add jupyter notebook cell ids in 4.5+ if missing
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: generate-cell-uuid
created_at: 2023-08-24T16:16:01Z
updated_at: 2023-08-25T08:53:18Z
url: https://github.com/astral-sh/ruff/pull/6853
synced_at: 2026-01-12T02:45:38Z
```

# Add jupyter notebook cell ids in 4.5+ if missing

---

_Pull request opened by @konstin on 2023-08-24 16:16_

**Summary** See https://github.com/astral-sh/ruff/issues/6834#issuecomment-1691202417

**Test Plan** Added a new notebook


---

_Review requested from @dhruvmanila by @konstin on 2023-08-24 16:16_

---

_@dhruvmanila approved on 2023-08-24 16:26_

Thanks!

---

_Comment by @github-actions[bot] on 2023-08-24 16:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.8±0.05ms     8.5 MB/sec    1.00      4.8±0.03ms     8.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1010.2±6.33µs    16.5 MB/sec    1.00  1006.4±13.54µs    16.5 MB/sec
formatter/numpy/globals.py                 1.00     98.6±1.24µs    29.9 MB/sec    1.01     99.9±0.43µs    29.5 MB/sec
formatter/pydantic/types.py                1.01  1991.6±14.03µs    12.8 MB/sec    1.00  1976.3±15.63µs    12.9 MB/sec
linter/all-rules/large/dataset.py          1.01     12.2±0.09ms     3.3 MB/sec    1.00     12.1±0.15ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.3±0.03ms     5.0 MB/sec    1.00      3.2±0.05ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.02    464.1±1.85µs     6.4 MB/sec    1.00    457.1±5.11µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.5±0.19ms     3.9 MB/sec    1.00      6.2±0.07ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      6.5±0.04ms     6.3 MB/sec    1.00      6.4±0.06ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1438.0±3.07µs    11.6 MB/sec    1.00   1438.9±8.27µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    168.9±5.79µs    17.5 MB/sec    1.00    167.0±1.50µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.8 MB/sec    1.00      2.9±0.02ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.6±0.11ms     7.3 MB/sec    1.03      5.8±0.19ms     7.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1169.2±34.38µs    14.2 MB/sec    1.02  1193.7±42.14µs    13.9 MB/sec
formatter/numpy/globals.py                 1.00    114.1±5.74µs    25.9 MB/sec    1.02    116.5±6.12µs    25.3 MB/sec
formatter/pydantic/types.py                1.00      2.3±0.08ms    11.0 MB/sec    1.02      2.4±0.06ms    10.8 MB/sec
linter/all-rules/large/dataset.py          1.01     15.4±0.49ms     2.6 MB/sec    1.00     15.3±0.29ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.10ms     4.0 MB/sec    1.01      4.2±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   528.6±21.34µs     5.6 MB/sec    1.00   529.7±18.24µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.18ms     3.2 MB/sec    1.00      7.9±0.17ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.13ms     4.8 MB/sec    1.02      8.6±0.12ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1814.5±51.19µs     9.2 MB/sec    1.02  1853.6±41.62µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    214.8±7.15µs    13.7 MB/sec    1.02    219.3±8.44µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.18ms     6.7 MB/sec    1.01      3.9±0.11ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Closed by @konstin on 2023-08-25 08:24_

---

_Reopened by @konstin on 2023-08-25 08:24_

---

_Merged by @konstin on 2023-08-25 08:34_

---

_Closed by @konstin on 2023-08-25 08:34_

---

_Branch deleted on 2023-08-25 08:34_

---
