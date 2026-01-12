```yaml
number: 6036
title: "Add additional test cases for `F823`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/F823
created_at: 2023-07-24T15:40:15Z
updated_at: 2023-07-24T16:12:26Z
url: https://github.com/astral-sh/ruff/pull/6036
synced_at: 2026-01-12T15:55:20Z
```

# Add additional test cases for `F823`

---

_@charliermarsh_

Making some behavior explicit / codified. See: https://github.com/astral-sh/ruff/issues/6029.


---

_Label `internal` added by @charliermarsh on 2023-07-24 15:40_

---

_Merged by @charliermarsh on 2023-07-24 15:49_

---

_Closed by @charliermarsh on 2023-07-24 15:49_

---

_Branch deleted on 2023-07-24 15:49_

---

_Comment by @github-actions[bot] on 2023-07-24 15:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.12ms     3.8 MB/sec    1.00     10.8±0.09ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.04ms     7.6 MB/sec    1.01      2.2±0.01ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    245.1±6.42µs    12.0 MB/sec    1.01    247.2±4.20µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.10ms     5.5 MB/sec    1.03      4.8±0.05ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.02     15.3±0.16ms     2.7 MB/sec    1.00     15.1±0.33ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.03ms     4.3 MB/sec    1.00      3.9±0.08ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.9±3.69µs     5.9 MB/sec    1.02    509.7±4.52µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.9±0.06ms     3.7 MB/sec    1.00      6.8±0.15ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.05ms     5.2 MB/sec    1.00      7.8±0.06ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1650.9±27.69µs    10.1 MB/sec    1.02  1686.8±16.37µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.3±4.02µs    16.3 MB/sec    1.04    188.1±1.81µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.06ms     7.3 MB/sec    1.02      3.5±0.04ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.18ms     3.7 MB/sec    1.00     11.1±0.14ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.04ms     7.6 MB/sec    1.01      2.2±0.06ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00    244.7±4.80µs    12.1 MB/sec    1.02    250.2±8.16µs    11.8 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.4 MB/sec    1.02      4.8±0.10ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.16ms     2.7 MB/sec    1.02     15.6±0.19ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.02      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    483.3±7.22µs     6.1 MB/sec    1.00    484.5±6.60µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.6 MB/sec    1.01      7.1±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.09ms     5.0 MB/sec    1.00      8.1±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1681.2±18.79µs     9.9 MB/sec    1.00  1676.3±19.69µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.7±3.56µs    15.4 MB/sec    1.00    191.7±2.85µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.1 MB/sec    1.00      3.6±0.05ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
