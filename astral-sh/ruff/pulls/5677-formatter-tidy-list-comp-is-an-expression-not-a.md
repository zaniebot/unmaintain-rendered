```yaml
number: 5677
title: "formatter: tidy: list_comp is an expression, not a statement"
type: pull_request
state: merged
author: davidszotten
labels: []
assignees: []
merged: true
base: main
head: format-list-comp-is-an-expr
created_at: 2023-07-11T07:52:21Z
updated_at: 2023-07-11T08:19:07Z
url: https://github.com/astral-sh/ruff/pull/5677
synced_at: 2026-01-12T03:36:55Z
```

# formatter: tidy: list_comp is an expression, not a statement

---

_Pull request opened by @davidszotten on 2023-07-11 07:52_

put the test fixture in the right folder

---

_@MichaReiser approved on 2023-07-11 07:53_

---

_Marked ready for review by @MichaReiser on 2023-07-11 07:53_

---

_Merged by @MichaReiser on 2023-07-11 08:00_

---

_Closed by @MichaReiser on 2023-07-11 08:00_

---

_Comment by @github-actions[bot] on 2023-07-11 08:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.03ms     5.1 MB/sec    1.02      8.2±0.04ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1835.3±1.92µs     9.1 MB/sec    1.01   1854.7±3.43µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    203.6±0.34µs    14.5 MB/sec    1.02    206.8±6.25µs    14.3 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.4 MB/sec    1.00      3.9±0.01ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.12ms     3.0 MB/sec    1.00     13.7±0.11ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.9±0.59µs     6.8 MB/sec    1.00    432.8±1.14µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.03ms     4.2 MB/sec    1.00      6.0±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.0 MB/sec    1.00      6.7±0.04ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1466.1±6.29µs    11.4 MB/sec    1.00   1461.7±3.82µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.3±0.29µs    17.7 MB/sec    1.00    166.9±1.80µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     12.8±0.54ms     3.2 MB/sec    1.00     12.3±0.60ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.7±0.13ms     6.1 MB/sec    1.00      2.7±0.10ms     6.2 MB/sec
formatter/numpy/globals.py                 1.00   307.4±12.63µs     9.6 MB/sec    1.01   310.8±14.33µs     9.5 MB/sec
formatter/pydantic/types.py                1.00      5.9±0.27ms     4.3 MB/sec    1.02      6.0±0.23ms     4.3 MB/sec
linter/all-rules/large/dataset.py          1.00     21.5±1.09ms  1939.1 KB/sec    1.00     21.6±0.59ms  1932.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.24ms     3.2 MB/sec    1.05      5.5±0.15ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   642.4±20.96µs     4.6 MB/sec    1.02   653.4±45.08µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.2±0.33ms     2.8 MB/sec    1.02      9.4±0.35ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.27ms     4.0 MB/sec    1.05     10.6±0.35ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.08ms     7.8 MB/sec    1.08      2.3±0.08ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   262.3±13.30µs    11.3 MB/sec    1.00   263.6±10.24µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.32ms     5.6 MB/sec    1.07      4.9±0.28ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
