```yaml
number: 4031
title: "Remove TODO in `handle_node_store`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/todo
created_at: 2023-04-19T19:19:53Z
updated_at: 2023-04-19T19:53:01Z
url: https://github.com/astral-sh/ruff/pull/4031
synced_at: 2026-01-12T15:55:14Z
```

# Remove TODO in `handle_node_store`

---

_@charliermarsh_

I don't think this is relevant, since within a comprehension, assignments are part of the comprehension scope anyway.

---

_Renamed from "Remove TODO in handle_node_store" to "Remove TODO in `handle_node_store`" by @charliermarsh on 2023-04-19 19:19_

---

_Merged by @charliermarsh on 2023-04-19 19:28_

---

_Closed by @charliermarsh on 2023-04-19 19:28_

---

_Branch deleted on 2023-04-19 19:28_

---

_Comment by @github-actions[bot] on 2023-04-19 19:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.03ms     2.9 MB/sec    1.01     14.2±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    458.1±1.74µs     6.4 MB/sec    1.01    460.4±1.91µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.7 MB/sec    1.00      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1603.5±2.81µs    10.4 MB/sec    1.02   1640.8±3.05µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.7±2.32µs    16.8 MB/sec    1.04    182.6±0.87µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.8 MB/sec    1.01      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.01      5.8±0.00ms     7.1 MB/sec    1.00      5.7±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1140.2±0.62µs    14.6 MB/sec    1.00   1140.9±1.08µs    14.6 MB/sec
parser/numpy/globals.py                    1.00    113.5±0.13µs    26.0 MB/sec    1.00    113.7±0.74µs    26.0 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.40ms     2.5 MB/sec    1.00     16.3±0.41ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     4.0 MB/sec    1.00      4.2±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   459.3±18.45µs     6.4 MB/sec    1.00    458.4±7.22µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.04ms     3.7 MB/sec    1.05      7.2±0.34ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.13ms     4.9 MB/sec    1.01      8.3±0.09ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1784.4±22.58µs     9.3 MB/sec    1.04  1861.2±46.39µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.8±1.31µs    15.7 MB/sec    1.00    186.9±2.84µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.09ms     6.7 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.6±0.02ms     6.2 MB/sec    1.03      6.8±0.12ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1292.3±25.14µs    12.9 MB/sec    1.01  1299.8±18.48µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    126.9±1.84µs    23.3 MB/sec    1.02    128.8±2.13µs    22.9 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.1 MB/sec    1.01      2.8±0.01ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
