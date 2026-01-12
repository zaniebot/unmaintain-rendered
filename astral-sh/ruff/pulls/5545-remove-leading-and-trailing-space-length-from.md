```yaml
number: 5545
title: "Remove leading and trailing space length from `Directive`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/noqa-range
created_at: 2023-07-05T22:51:14Z
updated_at: 2023-07-05T23:29:32Z
url: https://github.com/astral-sh/ruff/pull/5545
synced_at: 2026-01-12T03:36:55Z
```

# Remove leading and trailing space length from `Directive`

---

_Pull request opened by @charliermarsh on 2023-07-05 22:51_

## Summary

We only need this in one place (when removing the directive), and it simplifies a lot of details to just compute it there.


---

_Renamed from "Remove leading and trailing space length from Directive" to "Remove leading and trailing space length from `Directive`" by @charliermarsh on 2023-07-05 22:51_

---

_Merged by @charliermarsh on 2023-07-05 23:03_

---

_Closed by @charliermarsh on 2023-07-05 23:03_

---

_Branch deleted on 2023-07-05 23:03_

---

_Comment by @github-actions[bot] on 2023-07-05 23:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.2±0.07ms     4.4 MB/sec    1.00      9.1±0.07ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.03ms     8.1 MB/sec    1.00      2.0±0.03ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    228.2±2.98µs    12.9 MB/sec    1.01    230.8±4.04µs    12.8 MB/sec
formatter/pydantic/types.py                1.02      4.4±0.05ms     5.7 MB/sec    1.00      4.4±0.04ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.06ms     2.6 MB/sec    1.00     15.9±0.14ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.0±0.01ms     4.1 MB/sec    1.00      3.9±0.06ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.02    510.3±5.73µs     5.8 MB/sec    1.00    501.4±6.58µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.10ms     3.7 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.0±0.03ms     5.1 MB/sec    1.00      7.9±0.10ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1752.4±12.44µs     9.5 MB/sec    1.00  1722.3±14.70µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    197.4±2.64µs    15.0 MB/sec    1.00    196.3±2.31µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.1 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.09     13.5±0.56ms     3.0 MB/sec    1.00     12.4±0.44ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.8±0.16ms     6.0 MB/sec    1.00      2.7±0.08ms     6.3 MB/sec
formatter/numpy/globals.py                 1.06   322.9±15.96µs     9.1 MB/sec    1.00   305.6±40.90µs     9.7 MB/sec
formatter/pydantic/types.py                1.11      6.4±0.32ms     4.0 MB/sec    1.00      5.7±0.23ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.04     21.1±0.79ms  1978.8 KB/sec    1.00     20.2±0.70ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.4±0.26ms     3.1 MB/sec    1.00      5.3±0.22ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.05   649.8±23.60µs     4.5 MB/sec    1.00   616.6±20.84µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.29ms     2.8 MB/sec    1.01      9.1±0.36ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.6±0.36ms     3.8 MB/sec    1.03     11.0±0.48ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.07ms     7.5 MB/sec    1.00      2.2±0.07ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   263.6±10.88µs    11.2 MB/sec    1.01   265.9±14.13µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.28ms     5.4 MB/sec    1.02      4.8±0.30ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
