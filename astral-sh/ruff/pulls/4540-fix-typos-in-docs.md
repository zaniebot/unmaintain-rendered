```yaml
number: 4540
title: Fix typos in docs
type: pull_request
state: merged
author: Mr-Pepe
labels: []
assignees: []
merged: true
base: main
head: docs/fix-typo
created_at: 2023-05-20T10:00:46Z
updated_at: 2023-05-20T11:23:17Z
url: https://github.com/astral-sh/ruff/pull/4540
synced_at: 2026-01-12T03:50:03Z
```

# Fix typos in docs

---

_Pull request opened by @Mr-Pepe on 2023-05-20 10:00_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-05-20 10:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.2±0.06ms     2.9 MB/sec    1.00     14.0±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     4.8 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    422.3±1.19µs     7.0 MB/sec    1.00    420.9±0.86µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.04ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      6.8±0.01ms     6.0 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1469.0±3.78µs    11.3 MB/sec    1.00   1450.6±2.39µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    162.9±0.28µs    18.1 MB/sec    1.00    161.1±0.30µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.5 MB/sec    1.01      5.4±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.01   1070.8±0.68µs    15.5 MB/sec    1.00   1059.7±1.77µs    15.7 MB/sec
parser/numpy/globals.py                    1.01    109.9±0.22µs    26.8 MB/sec    1.00    108.8±0.25µs    27.1 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.5±0.30ms     2.1 MB/sec    1.02     19.8±0.32ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.09ms     3.4 MB/sec    1.02      5.0±0.09ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   576.2±13.00µs     5.1 MB/sec    1.02   586.3±12.56µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.14ms     3.1 MB/sec    1.02      8.4±0.14ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.6±0.15ms     4.2 MB/sec    1.02      9.8±0.14ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.04ms     8.2 MB/sec    1.03      2.1±0.04ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    226.9±6.85µs    13.0 MB/sec    1.06    239.4±9.41µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.08ms     5.9 MB/sec    1.02      4.4±0.07ms     5.7 MB/sec
parser/large/dataset.py                    1.00      7.8±0.10ms     5.2 MB/sec    1.05      8.2±0.08ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1482.1±28.92µs    11.2 MB/sec    1.06  1564.9±26.41µs    10.6 MB/sec
parser/numpy/globals.py                    1.00    152.3±4.42µs    19.4 MB/sec    1.05    159.2±2.93µs    18.5 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.06ms     7.6 MB/sec    1.04      3.5±0.05ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-20 11:23_

---

_Closed by @charliermarsh on 2023-05-20 11:23_

---
