```yaml
number: 4855
title: "Invert structure of Scope#shadowed_bindings"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/shadow
created_at: 2023-06-05T01:55:53Z
updated_at: 2023-06-05T02:23:38Z
url: https://github.com/astral-sh/ruff/pull/4855
synced_at: 2026-01-12T03:50:04Z
```

# Invert structure of Scope#shadowed_bindings

---

_Pull request opened by @charliermarsh on 2023-06-05 01:55_

## Summary

Rather than storing a map from name to list of shadowed bindings, we instead store a map from binding to binding that it shadows. This lets us determine, elsewhere, which binding shadowed which (instead of merely being able to determine all shadowed bindings for a name).


---

_Label `internal` added by @charliermarsh on 2023-06-05 01:56_

---

_Merged by @charliermarsh on 2023-06-05 02:03_

---

_Closed by @charliermarsh on 2023-06-05 02:03_

---

_Branch deleted on 2023-06-05 02:03_

---

_Comment by @github-actions[bot] on 2023-06-05 02:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.03ms     2.8 MB/sec    1.00     14.7±0.03ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.7 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    366.0±1.75µs     8.1 MB/sec    1.00    364.0±1.06µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.01ms     5.6 MB/sec    1.00      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1532.7±2.57µs    10.9 MB/sec    1.00   1535.0±5.12µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.5±0.94µs    17.8 MB/sec    1.00    166.3±0.27µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.03ms     7.8 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
parser/large/dataset.py                    1.02      5.8±0.01ms     7.0 MB/sec    1.00      5.7±0.01ms     7.2 MB/sec
parser/numpy/ctypeslib.py                  1.02   1134.7±1.09µs    14.7 MB/sec    1.00   1113.0±1.10µs    15.0 MB/sec
parser/numpy/globals.py                    1.02    116.9±0.26µs    25.2 MB/sec    1.00    114.6±3.27µs    25.8 MB/sec
parser/pydantic/types.py                   1.02      2.5±0.00ms    10.3 MB/sec    1.00      2.4±0.01ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.0±0.16ms     2.4 MB/sec    1.00     16.9±0.17ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     3.9 MB/sec    1.00      4.2±0.09ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    497.1±8.48µs     5.9 MB/sec    1.00    497.8±7.55µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.13ms     3.6 MB/sec    1.00      7.1±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.4±0.06ms     4.9 MB/sec    1.00      8.2±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1771.2±16.68µs     9.4 MB/sec    1.00  1737.8±18.25µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    198.3±5.12µs    14.9 MB/sec    1.00    196.9±5.22µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.08ms     6.7 MB/sec    1.00      3.7±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.6±0.05ms     6.2 MB/sec    1.00      6.5±0.05ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.01  1242.2±18.41µs    13.4 MB/sec    1.00  1234.2±19.17µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    126.6±1.93µs    23.3 MB/sec    1.00    126.7±3.61µs    23.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.04ms     9.1 MB/sec    1.00      2.8±0.04ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
