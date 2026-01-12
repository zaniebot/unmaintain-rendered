```yaml
number: 4611
title: "Rename `ContextFlags` to `SemanticModelFlags`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/context
created_at: 2023-05-23T20:05:33Z
updated_at: 2023-05-23T21:47:08Z
url: https://github.com/astral-sh/ruff/pull/4611
synced_at: 2026-01-12T15:55:16Z
```

# Rename `ContextFlags` to `SemanticModelFlags`

---

_@charliermarsh_

We renamed `Context` to `SemanticModel`, but overlooked this struct.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-23 20:05_

---

_Comment by @github-actions[bot] on 2023-05-23 20:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     15.2±0.09ms     2.7 MB/sec    1.00     14.7±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    368.3±0.75µs     8.0 MB/sec    1.00    368.6±0.99µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.05ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.01ms     5.6 MB/sec    1.00      7.2±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1526.8±2.69µs    10.9 MB/sec    1.00   1516.0±5.77µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    164.9±0.37µs    17.9 MB/sec    1.00    162.8±0.22µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.04ms     7.8 MB/sec    1.00      3.2±0.00ms     7.9 MB/sec
parser/large/dataset.py                    1.00      5.7±0.00ms     7.1 MB/sec    1.00      5.7±0.02ms     7.2 MB/sec
parser/numpy/ctypeslib.py                  1.01   1124.9±2.29µs    14.8 MB/sec    1.00   1118.3±4.12µs    14.9 MB/sec
parser/numpy/globals.py                    1.01    115.8±0.20µs    25.5 MB/sec    1.00    115.0±0.19µs    25.7 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.02ms    10.4 MB/sec    1.00      2.4±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.19ms     2.5 MB/sec    1.03     17.1±0.14ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.01      4.2±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.7±6.82µs     6.0 MB/sec    1.01    494.2±6.22µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.08ms     3.7 MB/sec    1.03      7.1±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.06ms     5.1 MB/sec    1.08      8.7±0.05ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1686.3±21.00µs     9.9 MB/sec    1.06  1785.6±18.37µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.1±3.83µs    15.7 MB/sec    1.06    199.9±6.39µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.1 MB/sec    1.07      3.8±0.05ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.5±0.04ms     6.3 MB/sec    1.00      6.5±0.05ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1214.3±18.77µs    13.7 MB/sec    1.01  1229.1±12.30µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    122.4±3.22µs    24.1 MB/sec    1.02    124.9±1.97µs    23.6 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.03ms     9.3 MB/sec    1.01      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-05-23 21:34_

---

_Merged by @charliermarsh on 2023-05-23 21:47_

---

_Closed by @charliermarsh on 2023-05-23 21:47_

---

_Branch deleted on 2023-05-23 21:47_

---
