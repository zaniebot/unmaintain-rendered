```yaml
number: 6400
title: Remove outdated TODO
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/comment
created_at: 2023-08-07T18:23:44Z
updated_at: 2023-08-07T18:50:02Z
url: https://github.com/astral-sh/ruff/pull/6400
synced_at: 2026-01-12T02:52:04Z
```

# Remove outdated TODO

---

_Pull request opened by @charliermarsh on 2023-08-07 18:23_

See: https://github.com/astral-sh/ruff/pull/6376#discussion_r1285539278.

---

_Renamed from "Remove outdated TOOD" to "Remove outdated TODO" by @charliermarsh on 2023-08-07 18:23_

---

_Label `internal` added by @charliermarsh on 2023-08-07 18:23_

---

_Merged by @charliermarsh on 2023-08-07 18:33_

---

_Closed by @charliermarsh on 2023-08-07 18:33_

---

_Branch deleted on 2023-08-07 18:33_

---

_Comment by @github-actions[bot] on 2023-08-07 18:50_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.05ms     5.0 MB/sec    1.00      8.1±0.09ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1616.8±16.18µs    10.3 MB/sec    1.00  1620.4±28.73µs    10.3 MB/sec
formatter/numpy/globals.py                 1.01    183.7±5.04µs    16.1 MB/sec    1.00    182.3±3.45µs    16.2 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.02ms     7.5 MB/sec    1.00      3.4±0.05ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     10.0±0.02ms     4.1 MB/sec    1.00     10.0±0.03ms     4.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.00ms     6.2 MB/sec    1.00      2.7±0.02ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    378.4±0.43µs     7.8 MB/sec    1.00    376.5±0.41µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.6±0.03ms     5.5 MB/sec    1.00      4.6±0.01ms     5.5 MB/sec
linter/default-rules/large/dataset.py      1.00      5.3±0.01ms     7.7 MB/sec    1.00      5.2±0.01ms     7.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1137.3±2.50µs    14.6 MB/sec    1.00   1131.9±3.97µs    14.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    128.3±0.22µs    23.0 MB/sec    1.00    128.0±0.88µs    23.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.8 MB/sec    1.00      2.3±0.03ms    10.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.05ms     4.2 MB/sec    1.01      9.9±0.05ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1856.9±9.11µs     9.0 MB/sec    1.01  1866.5±17.84µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    190.5±1.97µs    15.5 MB/sec    1.02    194.9±7.02µs    15.1 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.02ms     6.1 MB/sec    1.01      4.2±0.05ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.08ms     3.2 MB/sec    1.00     12.5±0.07ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    359.4±6.66µs     8.2 MB/sec    1.00    359.8±5.07µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.04ms     4.4 MB/sec    1.01      5.9±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.05ms     6.0 MB/sec    1.01      6.8±0.04ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1369.6±12.83µs    12.2 MB/sec    1.01  1383.1±11.30µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    141.4±6.15µs    20.9 MB/sec    1.00    140.9±5.10µs    20.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.03ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
