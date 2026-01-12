```yaml
number: 4131
title: Add pygrep-hooks documentation
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: pygrep-hooks-docs
created_at: 2023-04-27T13:17:40Z
updated_at: 2023-05-09T22:15:03Z
url: https://github.com/astral-sh/ruff/pull/4131
synced_at: 2026-01-12T15:55:14Z
```

# Add pygrep-hooks documentation

---

_@tjkuson_

Add documentation for the pygrep-hooks rules. Completes the ruleset.

Related to #2646.

---

_Comment by @github-actions[bot] on 2023-04-27 13:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.04ms     2.8 MB/sec    1.02     15.0±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.6±4.48µs     8.1 MB/sec    1.00    365.4±3.18µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.01      6.2±0.05ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.01ms     5.5 MB/sec    1.01      7.6±0.03ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1553.6±1.61µs    10.7 MB/sec    1.02   1583.8±4.03µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.6±0.45µs    17.4 MB/sec    1.01    171.5±0.66µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.02      3.4±0.03ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.9±0.01ms     6.9 MB/sec    1.00      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1153.9±2.10µs    14.4 MB/sec    1.01   1167.8±1.63µs    14.3 MB/sec
parser/numpy/globals.py                    1.00    117.5±0.24µs    25.1 MB/sec    1.00    117.5±0.53µs    25.1 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.02      2.6±0.00ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.09     22.2±0.99ms  1877.4 KB/sec    1.00     20.4±0.96ms  2041.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.2±0.30ms     3.2 MB/sec    1.00      5.1±0.27ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   600.9±35.11µs     4.9 MB/sec    1.07   645.5±37.29µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.8±0.39ms     2.9 MB/sec    1.00      8.6±0.49ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.05     11.5±0.57ms     3.5 MB/sec    1.00     11.0±0.50ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.4±0.18ms     7.0 MB/sec    1.00      2.3±0.10ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   273.9±16.61µs    10.8 MB/sec    1.06   291.4±22.46µs    10.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.30ms     5.3 MB/sec    1.05      5.0±0.27ms     5.1 MB/sec
parser/large/dataset.py                    1.00      8.6±0.43ms     4.7 MB/sec    1.02      8.7±0.49ms     4.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1745.6±74.13µs     9.5 MB/sec    1.00  1743.7±77.78µs     9.5 MB/sec
parser/numpy/globals.py                    1.01   171.3±10.91µs    17.2 MB/sec    1.00   168.9±13.80µs    17.5 MB/sec
parser/pydantic/types.py                   1.02      3.7±0.15ms     6.8 MB/sec    1.00      3.7±0.17ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-27 18:33_

---

_Closed by @charliermarsh on 2023-04-27 18:33_

---

_Branch deleted on 2023-05-09 22:15_

---
