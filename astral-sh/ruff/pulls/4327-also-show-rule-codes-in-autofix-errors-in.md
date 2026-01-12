```yaml
number: 4327
title: Also show rule codes in autofix errors in production codes
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: show_rule_codes_in_release_too
created_at: 2023-05-09T19:54:44Z
updated_at: 2023-05-11T15:36:05Z
url: https://github.com/astral-sh/ruff/pull/4327
synced_at: 2026-01-12T15:55:15Z
```

# Also show rule codes in autofix errors in production codes

---

_@konstin_

I needed those changes for #4326

---

_Review requested from @MichaReiser by @konstin on 2023-05-09 19:54_

---

_@charliermarsh approved on 2023-05-09 19:56_

---

_Comment by @github-actions[bot] on 2023-05-09 20:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.09ms     2.9 MB/sec    1.00     14.0±0.11ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    411.8±0.51µs     7.2 MB/sec    1.01    413.9±1.53µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.9±0.01ms     5.9 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1487.9±2.84µs    11.2 MB/sec    1.00   1475.5±3.08µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.2±0.58µs    18.1 MB/sec    1.00    163.2±0.50µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.02ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.5 MB/sec    1.00      5.5±0.00ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1062.2±1.19µs    15.7 MB/sec    1.01   1069.9±1.08µs    15.6 MB/sec
parser/numpy/globals.py                    1.00    108.2±0.50µs    27.3 MB/sec    1.00    108.1±0.32µs    27.3 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.9±1.20ms     2.0 MB/sec    1.00     19.8±0.94ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.22ms     3.4 MB/sec    1.03      5.1±0.30ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   585.5±28.06µs     5.0 MB/sec    1.02   599.9±33.91µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.5±0.60ms     3.0 MB/sec    1.00      8.2±0.42ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.44ms     4.1 MB/sec    1.00      9.9±0.44ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.13ms     7.7 MB/sec    1.00      2.2±0.14ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   246.3±14.93µs    12.0 MB/sec    1.01   248.8±15.63µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.26ms     5.8 MB/sec    1.00      4.4±0.20ms     5.8 MB/sec
parser/large/dataset.py                    1.03      8.3±0.28ms     4.9 MB/sec    1.00      8.1±0.31ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.04  1606.2±60.05µs    10.4 MB/sec    1.00  1550.4±71.22µs    10.7 MB/sec
parser/numpy/globals.py                    1.00    162.8±9.44µs    18.1 MB/sec    1.02   166.5±19.40µs    17.7 MB/sec
parser/pydantic/types.py                   1.02      3.6±0.15ms     7.0 MB/sec    1.00      3.6±0.18ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @konstin on 2023-05-11 15:36_

---

_Closed by @konstin on 2023-05-11 15:36_

---

_Branch deleted on 2023-05-11 15:36_

---
