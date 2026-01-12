```yaml
number: 6340
title: "Add missing enable check for `bad-string-format-character`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/bad
created_at: 2023-08-04T13:19:26Z
updated_at: 2023-08-04T14:08:51Z
url: https://github.com/astral-sh/ruff/pull/6340
synced_at: 2026-01-12T02:52:04Z
```

# Add missing enable check for `bad-string-format-character`

---

_Pull request opened by @charliermarsh on 2023-08-04 13:19_

_No description provided._

---

_Renamed from "Add missing enable check for bad-string-format-character" to "Add missing enable check for `bad-string-format-character`" by @charliermarsh on 2023-08-04 13:19_

---

_Label `bug` added by @charliermarsh on 2023-08-04 13:19_

---

_Merged by @charliermarsh on 2023-08-04 13:27_

---

_Closed by @charliermarsh on 2023-08-04 13:27_

---

_Branch deleted on 2023-08-04 13:27_

---

_Comment by @github-actions[bot] on 2023-08-04 13:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.01ms     5.1 MB/sec    1.00      8.0±0.01ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1560.1±3.95µs    10.7 MB/sec    1.01   1578.7±6.45µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    168.6±0.37µs    17.5 MB/sec    1.01    169.9±0.19µs    17.4 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.03ms     7.4 MB/sec    1.00      3.4±0.01ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     11.1±0.01ms     3.7 MB/sec    1.00     11.1±0.01ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.8 MB/sec    1.01      2.9±0.01ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    314.7±1.33µs     9.4 MB/sec    1.19  374.8±197.90µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.01ms     5.0 MB/sec    1.05      5.3±0.98ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.3±0.01ms     7.6 MB/sec    1.02      5.4±0.19ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1103.1±2.28µs    15.1 MB/sec    1.01   1113.4±2.22µs    15.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    111.7±0.27µs    26.4 MB/sec    1.01    112.6±0.85µs    26.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.7 MB/sec    1.01      2.4±0.01ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.7±0.12ms     4.2 MB/sec    1.00      9.6±0.08ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.02  1892.7±24.91µs     8.8 MB/sec    1.00  1851.8±19.52µs     9.0 MB/sec
formatter/numpy/globals.py                 1.03    214.6±4.52µs    13.7 MB/sec    1.00    208.9±8.34µs    14.1 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.05ms     6.2 MB/sec    1.00      4.1±0.05ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.09ms     3.1 MB/sec    1.01     13.3±0.14ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.04ms     4.8 MB/sec    1.00      3.4±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.4±5.76µs     7.0 MB/sec    1.00   424.4±12.56µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.14ms     4.2 MB/sec    1.00      6.0±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.06ms     6.1 MB/sec    1.00      6.7±0.05ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1381.2±17.54µs    12.1 MB/sec    1.01  1398.8±20.18µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   158.1±12.97µs    18.7 MB/sec    1.00    158.5±3.70µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.03ms     8.6 MB/sec    1.01      3.0±0.03ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
