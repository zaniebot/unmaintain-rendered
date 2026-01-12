```yaml
number: 5359
title: "Update the `invalid-escape-sequence` rule"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/tweaks
created_at: 2023-06-25T22:10:38Z
updated_at: 2023-06-25T22:42:41Z
url: https://github.com/astral-sh/ruff/pull/5359
synced_at: 2026-01-12T03:36:55Z
```

# Update the `invalid-escape-sequence` rule

---

_Pull request opened by @charliermarsh on 2023-06-25 22:10_

Just a couple small tweaks based on reading the rule with fresh eyes and new best-practices.

---

_Merged by @charliermarsh on 2023-06-25 22:20_

---

_Closed by @charliermarsh on 2023-06-25 22:20_

---

_Branch deleted on 2023-06-25 22:20_

---

_Comment by @github-actions[bot] on 2023-06-25 22:42_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.7±0.03ms     6.1 MB/sec    1.00      6.6±0.01ms     6.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1560.1±1.37µs    10.7 MB/sec    1.00   1562.1±1.83µs    10.7 MB/sec
formatter/numpy/globals.py                 1.00    186.3±0.43µs    15.8 MB/sec    1.02    190.4±0.24µs    15.5 MB/sec
formatter/pydantic/types.py                1.01      3.5±0.00ms     7.3 MB/sec    1.00      3.4±0.02ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.02     13.4±0.10ms     3.0 MB/sec    1.00     13.1±0.05ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.4±0.01ms     4.9 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.03    435.8±0.66µs     6.8 MB/sec    1.00    424.0±0.50µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.01ms     4.3 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      6.7±0.02ms     6.1 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1485.6±1.80µs    11.2 MB/sec    1.00   1444.9±5.20µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.05    168.2±1.13µs    17.5 MB/sec    1.00    160.2±0.26µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.1±0.01ms     8.3 MB/sec    1.00      3.0±0.00ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.06ms     5.2 MB/sec    1.00      7.8±0.05ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1795.5±33.40µs     9.3 MB/sec    1.01  1812.0±22.53µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    220.3±7.35µs    13.4 MB/sec    1.02   224.0±12.46µs    13.2 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.06ms     6.3 MB/sec    1.00      4.1±0.05ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.5±0.14ms     2.6 MB/sec    1.00     15.3±0.15ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    501.3±6.08µs     5.9 MB/sec    1.00    500.0±5.43µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.06ms     3.7 MB/sec    1.00      6.8±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.05ms     5.1 MB/sec    1.00      7.9±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1724.3±17.91µs     9.7 MB/sec    1.00  1711.5±13.64µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.7±2.34µs    14.8 MB/sec    1.00    200.4±3.06µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.04ms     6.9 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
