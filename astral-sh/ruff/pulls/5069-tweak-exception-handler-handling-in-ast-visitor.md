```yaml
number: 5069
title: Tweak exception-handler handling in AST visitor
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/except
created_at: 2023-06-14T00:50:47Z
updated_at: 2023-06-14T01:17:34Z
url: https://github.com/astral-sh/ruff/pull/5069
synced_at: 2026-01-12T03:43:30Z
```

# Tweak exception-handler handling in AST visitor

---

_Pull request opened by @charliermarsh on 2023-06-14 00:50_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-14 00:50_

---

_Merged by @charliermarsh on 2023-06-14 01:00_

---

_Closed by @charliermarsh on 2023-06-14 01:00_

---

_Branch deleted on 2023-06-14 01:00_

---

_Comment by @github-actions[bot] on 2023-06-14 01:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1388.1±3.06µs    12.0 MB/sec    1.00   1386.7±5.60µs    12.0 MB/sec
formatter/numpy/globals.py                 1.00    136.5±0.60µs    21.6 MB/sec    1.00    136.6±0.27µs    21.6 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.3 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.05ms     2.7 MB/sec    1.01     15.0±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    369.6±7.10µs     8.0 MB/sec    1.00    369.3±2.20µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.05ms     4.0 MB/sec    1.01      6.4±0.03ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.03ms     5.5 MB/sec    1.02      7.6±0.03ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1543.7±3.62µs    10.8 MB/sec    1.01   1564.5±6.59µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.4±0.34µs    17.8 MB/sec    1.01    167.4±0.70µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.01      3.4±0.01ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.03ms     5.2 MB/sec    1.00      7.8±0.05ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1570.6±17.60µs    10.6 MB/sec    1.01  1584.2±17.40µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    154.5±1.04µs    19.1 MB/sec    1.01    156.6±7.79µs    18.8 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     8.0 MB/sec    1.00      3.2±0.04ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.3±0.06ms     2.5 MB/sec    1.04     17.0±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     3.9 MB/sec    1.02      4.3±0.02ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.3±6.51µs     6.8 MB/sec    1.02    441.7±5.85µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.03ms     3.6 MB/sec    1.04      7.4±0.10ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.07ms     4.9 MB/sec    1.09      9.0±0.08ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1730.8±21.61µs     9.6 MB/sec    1.07  1844.5±22.15µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.9±4.78µs    15.8 MB/sec    1.04    194.1±2.08µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.8 MB/sec    1.07      4.0±0.06ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
