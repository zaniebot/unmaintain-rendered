```yaml
number: 4224
title: Add docs for PLC rules
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: plc-docs
created_at: 2023-05-04T13:57:28Z
updated_at: 2023-05-09T22:15:03Z
url: https://github.com/astral-sh/ruff/pull/4224
synced_at: 2026-01-12T15:55:15Z
```

# Add docs for PLC rules

---

_@tjkuson_

Add documentation to the Pylint Convention (PLC) rules.

Related to #2646.

---

_Comment by @github-actions[bot] on 2023-05-04 14:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.08ms     2.4 MB/sec    1.00     17.1±0.10ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.1 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    502.7±2.16µs     5.9 MB/sec    1.00    499.1±1.41µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.05ms     3.6 MB/sec    1.00      7.1±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.03ms     4.9 MB/sec    1.00      8.3±0.04ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1786.8±2.76µs     9.3 MB/sec    1.00   1792.0±5.18µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.1±0.55µs    14.8 MB/sec    1.01    200.8±0.40µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.9 MB/sec    1.01      3.7±0.02ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.6±0.02ms     6.2 MB/sec    1.00      6.5±0.01ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.01   1279.6±1.10µs    13.0 MB/sec    1.00   1271.9±0.89µs    13.1 MB/sec
parser/numpy/globals.py                    1.01    130.9±0.27µs    22.5 MB/sec    1.00    129.2±0.25µs    22.8 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.00ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.0±0.57ms     2.0 MB/sec    1.00     20.0±0.46ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.1±0.19ms     3.3 MB/sec    1.00      5.0±0.18ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   604.8±35.30µs     4.9 MB/sec    1.01   608.5±30.64µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.29ms     3.0 MB/sec    1.00      8.4±0.28ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.01     10.2±0.24ms     4.0 MB/sec    1.00     10.1±0.27ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.08ms     7.7 MB/sec    1.00      2.1±0.07ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   247.4±14.54µs    11.9 MB/sec    1.01   250.1±15.39µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.19ms     5.6 MB/sec    1.00      4.5±0.15ms     5.6 MB/sec
parser/large/dataset.py                    1.03      8.3±0.23ms     4.9 MB/sec    1.00      8.1±0.16ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.01  1563.1±59.25µs    10.7 MB/sec    1.00  1548.0±55.19µs    10.8 MB/sec
parser/numpy/globals.py                    1.00    160.2±7.18µs    18.4 MB/sec    1.00    160.2±8.54µs    18.4 MB/sec
parser/pydantic/types.py                   1.01      3.5±0.12ms     7.3 MB/sec    1.00      3.5±0.13ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-05-04 14:26_

---

_Merged by @charliermarsh on 2023-05-04 14:56_

---

_Closed by @charliermarsh on 2023-05-04 14:56_

---

_Label `documentation` added by @charliermarsh on 2023-05-04 14:56_

---

_Branch deleted on 2023-05-09 22:15_

---
