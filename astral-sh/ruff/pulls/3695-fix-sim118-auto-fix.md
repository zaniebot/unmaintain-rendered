```yaml
number: 3695
title: Fix SIM118 auto-fix
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-sim118-auto-fix
created_at: 2023-03-23T20:49:45Z
updated_at: 2023-03-23T22:36:59Z
url: https://github.com/astral-sh/ruff/pull/3695
synced_at: 2026-01-12T04:39:45Z
```

# Fix SIM118 auto-fix

---

_Pull request opened by @JonathanPlasse on 2023-03-23 20:49_

- Close #3690

---

_Comment by @github-actions[bot] on 2023-03-23 21:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.7±0.04ms     3.0 MB/sec    1.01     13.9±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.01      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    495.9±1.16µs     5.9 MB/sec    1.01    498.8±0.92µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.03ms     4.2 MB/sec    1.01      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.03ms     5.6 MB/sec    1.04      7.5±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1626.0±4.04µs    10.2 MB/sec    1.02   1665.1±3.61µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.8±0.47µs    16.2 MB/sec    1.01    184.4±0.72µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.03      3.5±0.01ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.3±0.12ms     2.7 MB/sec    1.00     15.4±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.0 MB/sec    1.00      4.1±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    533.4±6.68µs     5.5 MB/sec    1.01    536.7±8.15µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.07ms     3.8 MB/sec    1.01      6.8±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.05ms     4.9 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1818.5±21.85µs     9.2 MB/sec    1.00  1820.4±17.43µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.2±8.11µs    14.6 MB/sec    1.00    202.7±6.33µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.6 MB/sec    1.00      3.9±0.06ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-23 21:14_

---

_Closed by @charliermarsh on 2023-03-23 21:14_

---

_Branch deleted on 2023-03-23 22:36_

---
