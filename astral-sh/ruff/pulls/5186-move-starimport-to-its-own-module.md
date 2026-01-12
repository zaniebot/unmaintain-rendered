```yaml
number: 5186
title: "Move `StarImport` to its own module"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/star-import
created_at: 2023-06-19T16:04:47Z
updated_at: 2023-06-20T17:25:15Z
url: https://github.com/astral-sh/ruff/pull/5186
synced_at: 2026-01-12T15:55:17Z
```

# Move `StarImport` to its own module

---

_@charliermarsh_

_No description provided._

---

_@MichaReiser approved on 2023-06-19 16:08_

---

_Comment by @MichaReiser on 2023-06-19 16:08_

I don't expect this to affect performance in a meaningful way but recomputing might be cheaper than writing more data.

---

_Comment by @github-actions[bot] on 2023-06-19 16:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.7±0.01ms     6.1 MB/sec    1.01      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1362.4±1.65µs    12.2 MB/sec    1.00   1368.3±2.21µs    12.2 MB/sec
formatter/numpy/globals.py                 1.00    132.6±0.23µs    22.3 MB/sec    1.01    133.8±0.93µs    22.0 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.01ms     9.3 MB/sec    1.01      2.8±0.00ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.01     13.8±0.01ms     2.9 MB/sec    1.00     13.7±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.01ms     4.8 MB/sec    1.00      3.4±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    367.3±5.37µs     8.0 MB/sec    1.00    363.0±1.51µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.8 MB/sec    1.00      7.0±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1500.7±2.68µs    11.1 MB/sec    1.00   1490.5±2.77µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    160.1±0.43µs    18.4 MB/sec    1.00    158.9±0.62µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.00ms     7.9 MB/sec    1.00      3.2±0.01ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.5±0.05ms     5.4 MB/sec    1.08      8.1±0.06ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1555.0±18.67µs    10.7 MB/sec    1.06  1654.7±19.93µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    155.6±3.86µs    19.0 MB/sec    1.06    165.2±4.94µs    17.9 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.04ms     8.3 MB/sec    1.08      3.3±0.05ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.01     15.2±0.11ms     2.7 MB/sec    1.00     15.0±0.13ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.03ms     4.2 MB/sec    1.00      4.0±0.05ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    499.9±6.62µs     5.9 MB/sec    1.00    495.7±5.85µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.06ms     3.8 MB/sec    1.00      6.7±0.05ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.02      8.0±0.07ms     5.1 MB/sec    1.00      7.8±0.05ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1719.8±16.71µs     9.7 MB/sec    1.00  1686.0±16.30µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    199.9±5.37µs    14.8 MB/sec    1.00    197.0±2.90µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.6±0.04ms     7.0 MB/sec    1.00      3.5±0.03ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-19 16:34_

Yeah I might roll this back actually.

---

_Renamed from "Store qualified name and range on `StarImport`" to "Move `StarImport` to its own module" by @charliermarsh on 2023-06-20 16:10_

---

_@konstin approved on 2023-06-20 17:04_

---

_Merged by @charliermarsh on 2023-06-20 17:12_

---

_Closed by @charliermarsh on 2023-06-20 17:12_

---

_Branch deleted on 2023-06-20 17:12_

---
