```yaml
number: 5568
title: "Fix formatter `StmtTry` test"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: fix_try_test
created_at: 2023-07-06T18:16:29Z
updated_at: 2023-07-06T18:42:27Z
url: https://github.com/astral-sh/ruff/pull/5568
synced_at: 2026-01-12T15:55:18Z
```

# Fix formatter `StmtTry` test

---

_@konstin_

For some reason this didn't turn up on CI before

CC @michareiser this is the fix for the error you had

---

_Merged by @konstin on 2023-07-06 18:23_

---

_Closed by @konstin on 2023-07-06 18:23_

---

_Branch deleted on 2023-07-06 18:23_

---

_Comment by @github-actions[bot] on 2023-07-06 18:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.03ms     4.3 MB/sec    1.00      9.4±0.07ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.07      2.2±0.12ms     7.5 MB/sec    1.00      2.1±0.03ms     8.0 MB/sec
formatter/numpy/globals.py                 1.05   247.8±23.46µs    11.9 MB/sec    1.00    235.2±1.05µs    12.5 MB/sec
formatter/pydantic/types.py                1.05      4.7±0.11ms     5.4 MB/sec    1.00      4.5±0.04ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.22ms     2.6 MB/sec    1.05     16.5±0.31ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.1 MB/sec    1.07      4.3±0.16ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02    521.8±1.84µs     5.7 MB/sec    1.00   511.1±11.89µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.05ms     3.6 MB/sec    1.00      7.2±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.03ms     5.0 MB/sec    1.00      8.1±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1766.4±6.75µs     9.4 MB/sec    1.00  1763.1±19.19µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    214.2±7.90µs    13.8 MB/sec    1.00   209.9±12.23µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.8±0.09ms     6.7 MB/sec    1.00      3.6±0.03ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.9±0.53ms     3.4 MB/sec    1.00     12.0±0.44ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.13ms     6.6 MB/sec    1.00      2.5±0.14ms     6.6 MB/sec
formatter/numpy/globals.py                 1.00   293.4±15.81µs    10.1 MB/sec    1.00   292.9±22.56µs    10.1 MB/sec
formatter/pydantic/types.py                1.04      5.7±0.33ms     4.5 MB/sec    1.00      5.5±0.27ms     4.6 MB/sec
linter/all-rules/large/dataset.py          1.00     20.5±0.86ms  2036.1 KB/sec    1.01     20.6±0.95ms  2024.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.3±0.21ms     3.1 MB/sec    1.00      5.3±0.19ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   631.2±31.42µs     4.7 MB/sec    1.01   634.9±29.26µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      9.1±0.42ms     2.8 MB/sec    1.00      8.9±0.31ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.02     10.5±0.42ms     3.9 MB/sec    1.00     10.3±0.50ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.08ms     7.6 MB/sec    1.00      2.2±0.07ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   261.9±13.54µs    11.3 MB/sec    1.02   267.9±14.01µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.21ms     5.4 MB/sec    1.00      4.7±0.20ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
