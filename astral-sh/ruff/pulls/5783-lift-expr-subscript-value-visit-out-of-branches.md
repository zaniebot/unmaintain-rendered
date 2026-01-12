```yaml
number: 5783
title: "Lift `Expr::Subscript` value visit out of branches"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/bind
created_at: 2023-07-15T18:54:56Z
updated_at: 2023-07-15T19:28:57Z
url: https://github.com/astral-sh/ruff/pull/5783
synced_at: 2026-01-12T03:30:21Z
```

# Lift `Expr::Subscript` value visit out of branches

---

_Pull request opened by @charliermarsh on 2023-07-15 18:54_

Like #5772, but for subscripts.

---

_Label `internal` added by @charliermarsh on 2023-07-15 18:55_

---

_Comment by @github-actions[bot] on 2023-07-15 19:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.6±0.05ms     4.2 MB/sec    1.00      9.4±0.06ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.02   1898.0±5.84µs     8.8 MB/sec    1.00   1865.3±6.41µs     8.9 MB/sec
formatter/numpy/globals.py                 1.01    211.5±0.28µs    14.0 MB/sec    1.00    210.3±0.76µs    14.0 MB/sec
formatter/pydantic/types.py                1.03      4.1±0.01ms     6.2 MB/sec    1.00      4.0±0.02ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.10ms     3.0 MB/sec    1.00     13.7±0.13ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     4.9 MB/sec    1.00      3.4±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    449.5±1.80µs     6.6 MB/sec    1.02    458.2±2.27µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.04ms     4.2 MB/sec    1.00      6.1±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.03ms     6.1 MB/sec    1.01      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1457.6±5.82µs    11.4 MB/sec    1.02   1491.5±2.49µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.0±0.28µs    17.8 MB/sec    1.02    170.1±1.76µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     11.4±0.28ms     3.6 MB/sec    1.00     11.2±0.15ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.2±0.04ms     7.6 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    248.0±7.33µs    11.9 MB/sec    1.01    249.5±7.83µs    11.8 MB/sec
formatter/pydantic/types.py                1.03      4.9±0.09ms     5.2 MB/sec    1.00      4.7±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.2±0.17ms     2.5 MB/sec    1.00     16.2±0.25ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     4.0 MB/sec    1.01      4.2±0.08ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    510.6±8.37µs     5.8 MB/sec    1.01   514.0±10.21µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.19ms     3.5 MB/sec    1.00      7.3±0.16ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.08ms     5.0 MB/sec    1.01      8.2±0.11ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1728.5±21.69µs     9.6 MB/sec    1.01  1751.8±22.34µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.3±3.63µs    14.4 MB/sec    1.00    203.7±4.00µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.01      3.7±0.05ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-15 19:07_

---

_Merged by @charliermarsh on 2023-07-15 19:12_

---

_Closed by @charliermarsh on 2023-07-15 19:12_

---

_Branch deleted on 2023-07-15 19:12_

---
