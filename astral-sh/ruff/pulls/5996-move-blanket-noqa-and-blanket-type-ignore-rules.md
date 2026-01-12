```yaml
number: 5996
title: "Move blanket `noqa` and blanket `type: ignore` rules into token-based checker"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/comment
created_at: 2023-07-23T00:59:57Z
updated_at: 2023-07-23T01:42:56Z
url: https://github.com/astral-sh/ruff/pull/5996
synced_at: 2026-01-12T03:30:22Z
```

# Move blanket `noqa` and blanket `type: ignore` rules into token-based checker

---

_Pull request opened by @charliermarsh on 2023-07-23 00:59_

Closes https://github.com/astral-sh/ruff/issues/5981.


---

_Label `internal` added by @charliermarsh on 2023-07-23 01:00_

---

_Comment by @github-actions[bot] on 2023-07-23 01:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      9.5±0.32ms     4.3 MB/sec    1.00      9.2±0.30ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.02  1883.7±70.47µs     8.8 MB/sec    1.00  1854.9±75.81µs     9.0 MB/sec
formatter/numpy/globals.py                 1.03    205.7±9.62µs    14.3 MB/sec    1.00    200.2±8.49µs    14.7 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.17ms     6.2 MB/sec    1.00      4.0±0.16ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.39ms     3.1 MB/sec    1.02     13.3±0.34ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.10ms     5.0 MB/sec    1.01      3.3±0.10ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   430.1±23.15µs     6.9 MB/sec    1.06   458.0±19.53µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.17ms     4.4 MB/sec    1.03      6.0±0.19ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.24ms     6.1 MB/sec    1.00      6.7±0.18ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1436.9±55.73µs    11.6 MB/sec    1.00  1427.6±50.68µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.9±7.20µs    18.7 MB/sec    1.00    157.4±9.12µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.12ms     8.3 MB/sec    1.00      3.0±0.13ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.5±0.40ms     3.0 MB/sec    1.00     13.6±0.58ms     3.0 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.7±0.14ms     6.1 MB/sec    1.00      2.7±0.11ms     6.1 MB/sec
formatter/numpy/globals.py                 1.00   301.5±16.37µs     9.8 MB/sec    1.02   307.2±18.38µs     9.6 MB/sec
formatter/pydantic/types.py                1.02      5.9±0.32ms     4.3 MB/sec    1.00      5.8±0.21ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     19.6±0.50ms     2.1 MB/sec    1.01     19.8±0.65ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.2±0.29ms     3.2 MB/sec    1.00      5.1±0.21ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.02   622.7±29.81µs     4.7 MB/sec    1.00   611.4±25.33µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.04      8.9±0.31ms     2.9 MB/sec    1.00      8.6±0.29ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.24ms     4.1 MB/sec    1.03     10.2±0.57ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.1±0.10ms     7.8 MB/sec    1.00      2.1±0.13ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   238.7±14.73µs    12.4 MB/sec    1.01   240.2±16.56µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.22ms     5.7 MB/sec    1.00      4.5±0.17ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-23 01:22_

---

_Closed by @charliermarsh on 2023-07-23 01:22_

---

_Branch deleted on 2023-07-23 01:22_

---
