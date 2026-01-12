```yaml
number: 3968
title: Add multi-edit change to BREAKING_CHANGES.md
type: pull_request
state: merged
author: charliermarsh
labels:
  - breaking
assignees: []
merged: true
base: main
head: charlie/breaking
created_at: 2023-04-13T23:04:07Z
updated_at: 2023-04-13T23:28:27Z
url: https://github.com/astral-sh/ruff/pull/3968
synced_at: 2026-01-12T04:28:19Z
```

# Add multi-edit change to BREAKING_CHANGES.md

---

_Pull request opened by @charliermarsh on 2023-04-13 23:04_

See: #3709.

---

_Label `breaking` added by @charliermarsh on 2023-04-13 23:04_

---

_Merged by @charliermarsh on 2023-04-13 23:12_

---

_Closed by @charliermarsh on 2023-04-13 23:12_

---

_Branch deleted on 2023-04-13 23:12_

---

_Comment by @github-actions[bot] on 2023-04-13 23:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.0±0.70ms     2.3 MB/sec    1.04     18.7±0.64ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.6±0.21ms     3.6 MB/sec    1.00      4.5±0.35ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   594.7±15.35µs     5.0 MB/sec    1.00   586.9±22.55µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.32ms     3.3 MB/sec    1.04      8.1±0.28ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.4±0.38ms     4.3 MB/sec    1.00      9.4±0.38ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.07ms     8.1 MB/sec    1.00      2.0±0.10ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   240.3±13.18µs    12.3 MB/sec    1.01    243.3±8.11µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.14ms     5.9 MB/sec    1.02      4.4±0.20ms     5.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     15.3±0.37ms     2.7 MB/sec    1.00     15.1±0.40ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.8±0.08ms     4.3 MB/sec    1.00      3.8±0.06ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    450.9±9.10µs     6.5 MB/sec    1.00   450.9±12.06µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.10ms     4.0 MB/sec    1.00      6.3±0.11ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.02      7.5±0.09ms     5.4 MB/sec    1.00      7.4±0.10ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1645.4±31.15µs    10.1 MB/sec    1.00  1626.3±36.26µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.3±5.61µs    16.7 MB/sec    1.00    176.2±4.93µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.07ms     7.4 MB/sec    1.00      3.4±0.08ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
