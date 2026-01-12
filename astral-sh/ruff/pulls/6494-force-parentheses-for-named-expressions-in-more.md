```yaml
number: 6494
title: Force parentheses for named expressions in more contexts
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/named
created_at: 2023-08-11T05:03:11Z
updated_at: 2023-08-11T05:54:48Z
url: https://github.com/astral-sh/ruff/pull/6494
synced_at: 2026-01-12T15:55:21Z
```

# Force parentheses for named expressions in more contexts

---

_@charliermarsh_

See: https://github.com/astral-sh/ruff/pull/6436#issuecomment-1673583888.


---

_Marked ready for review by @charliermarsh on 2023-08-11 05:03_

---

_Label `formatter` added by @charliermarsh on 2023-08-11 05:03_

---

_Review requested from @konstin by @charliermarsh on 2023-08-11 05:03_

---

_Comment by @github-actions[bot] on 2023-08-11 05:35_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.04ms     4.9 MB/sec    1.00      8.3±0.09ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1632.8±11.59µs    10.2 MB/sec    1.01  1645.5±30.26µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    183.9±3.00µs    16.0 MB/sec    1.00    184.4±3.59µs    16.0 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.03ms     7.3 MB/sec    1.01      3.5±0.04ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.06ms     3.9 MB/sec    1.01     10.5±0.06ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     6.0 MB/sec    1.01      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    395.3±2.70µs     7.5 MB/sec    1.02    402.3±2.01µs     7.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.02ms     4.7 MB/sec    1.01      5.4±0.03ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.5 MB/sec    1.01      5.5±0.02ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1230.9±30.92µs    13.5 MB/sec    1.00  1219.4±22.97µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    142.9±4.69µs    20.6 MB/sec    1.01    144.1±3.22µs    20.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.4 MB/sec    1.01      2.5±0.04ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.05ms     4.0 MB/sec    1.03     10.5±0.03ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1922.2±15.05µs     8.7 MB/sec    1.04  1990.3±49.22µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    199.7±2.81µs    14.8 MB/sec    1.02    203.9±5.74µs    14.5 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.03ms     5.9 MB/sec    1.03      4.4±0.04ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.01     12.8±0.08ms     3.2 MB/sec    1.00     12.7±0.05ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    374.3±4.76µs     7.9 MB/sec    1.00    372.8±6.62µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.6±0.06ms     3.8 MB/sec    1.00      6.6±0.03ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.05ms     5.8 MB/sec    1.00      6.9±0.05ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1459.2±13.06µs    11.4 MB/sec    1.00  1449.4±12.14µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    148.7±1.74µs    19.8 MB/sec    1.00    148.5±1.10µs    19.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.1 MB/sec    1.00      3.1±0.02ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-08-11 05:44_

Makes sense if these are syntax errors. I'm a bit confused of where Black is handling this because these expressions aren't part of 

https://github.com/psf/black/blob/01b8d3d4095ebdb91d0d39012a517931625c63cb/src/black/linegen.py#L1385-L1398

---

_Comment by @charliermarsh on 2023-08-11 05:54_

I’m not sure either. Our list was based off of that source.

---

_Merged by @charliermarsh on 2023-08-11 05:54_

---

_Closed by @charliermarsh on 2023-08-11 05:54_

---

_Branch deleted on 2023-08-11 05:54_

---
