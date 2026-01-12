```yaml
number: 3740
title: Fix SIM222 and SIM223 false negative
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - rule
assignees: []
merged: true
base: main
head: fix-sim222-and-sim223-false-negative
created_at: 2023-03-26T13:49:38Z
updated_at: 2023-03-26T18:28:33Z
url: https://github.com/astral-sh/ruff/pull/3740
synced_at: 2026-01-12T04:39:45Z
```

# Fix SIM222 and SIM223 false negative

---

_Pull request opened by @JonathanPlasse on 2023-03-26 13:49_

- Mentioned in #3732

---

_Comment by @github-actions[bot] on 2023-03-26 14:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.5±0.01ms     3.0 MB/sec    1.02     13.7±0.29ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    493.8±0.91µs     6.0 MB/sec    1.00    495.1±0.69µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.03ms     4.3 MB/sec    1.00      6.0±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.7 MB/sec    1.00      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1638.9±1.67µs    10.2 MB/sec    1.00   1641.5±3.52µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.1±0.36µs    15.9 MB/sec    1.00    185.7±0.41µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.05     16.1±0.28ms     2.5 MB/sec    1.00     15.3±0.37ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.10ms     4.0 MB/sec    1.00      4.1±0.07ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    531.7±7.50µs     5.5 MB/sec    1.01   535.6±10.27µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.19ms     3.7 MB/sec    1.00      6.9±0.14ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.11ms     4.9 MB/sec    1.01      8.3±0.15ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1813.8±26.30µs     9.2 MB/sec    1.03  1861.8±21.97µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.6±3.81µs    14.6 MB/sec    1.02    204.8±5.47µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.06ms     6.7 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-03-26 18:04_

---

_Merged by @charliermarsh on 2023-03-26 18:09_

---

_Closed by @charliermarsh on 2023-03-26 18:09_

---

_Branch deleted on 2023-03-26 18:28_

---
