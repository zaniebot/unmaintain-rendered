```yaml
number: 3889
title: Allow legacy C and T selectors in JSON schema
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rules
created_at: 2023-04-05T17:53:27Z
updated_at: 2023-04-05T18:17:18Z
url: https://github.com/astral-sh/ruff/pull/3889
synced_at: 2026-01-12T15:55:14Z
```

# Allow legacy C and T selectors in JSON schema

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-05 17:58_

---

_Closed by @charliermarsh on 2023-04-05 17:58_

---

_Branch deleted on 2023-04-05 17:58_

---

_Comment by @github-actions[bot] on 2023-04-05 18:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.4±0.09ms     2.8 MB/sec    1.01     14.5±0.16ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    458.5±0.79µs     6.4 MB/sec    1.00    457.4±1.06µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.06ms     5.6 MB/sec    1.00      7.2±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1641.9±2.11µs    10.1 MB/sec    1.00   1633.1±5.38µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    181.6±0.59µs    16.2 MB/sec    1.00    180.0±0.96µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.1±0.10ms     2.5 MB/sec    1.00     16.0±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.06ms     4.0 MB/sec    1.00      4.1±0.01ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.6±7.34µs     6.8 MB/sec    1.00    433.6±7.18µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.03ms     3.7 MB/sec    1.00      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.03ms     5.0 MB/sec    1.00      8.2±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1773.4±13.93µs     9.4 MB/sec    1.02  1804.4±22.59µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.9±5.33µs    15.7 MB/sec    1.01    188.9±4.79µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.8 MB/sec    1.00      3.8±0.03ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
