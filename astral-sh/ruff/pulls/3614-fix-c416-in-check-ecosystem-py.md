```yaml
number: 3614
title: Fix C416 in check_ecosystem.py
type: pull_request
state: closed
author: JonathanPlasse
labels: []
assignees: []
base: main
head: fix-pre-commit
created_at: 2023-03-19T20:36:48Z
updated_at: 2023-03-19T21:01:54Z
url: https://github.com/astral-sh/ruff/pull/3614
synced_at: 2026-01-12T04:39:45Z
```

# Fix C416 in check_ecosystem.py

---

_Pull request opened by @JonathanPlasse on 2023-03-19 20:36_

I ran pre-commit.

---

_Closed by @JonathanPlasse on 2023-03-19 20:48_

---

_Comment by @JonathanPlasse on 2023-03-19 20:48_

- Close in favour of #3615 

---

_Branch deleted on 2023-03-19 20:48_

---

_Comment by @github-actions[bot] on 2023-03-19 20:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.53ms     2.8 MB/sec    1.00     14.8±0.49ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.9±0.09ms     4.2 MB/sec    1.00      3.8±0.14ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.06   551.7±15.15µs     5.3 MB/sec    1.00   518.7±26.30µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.6±0.13ms     3.8 MB/sec    1.00      6.4±0.28ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.35ms     5.0 MB/sec    1.02      8.2±0.27ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1798.9±70.33µs     9.3 MB/sec    1.04  1868.0±68.69µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.2±8.61µs    15.4 MB/sec    1.06    202.0±8.05µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.14ms     6.9 MB/sec    1.05      3.9±0.12ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.6±0.08ms     2.8 MB/sec    1.00     14.6±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.00      4.0±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    522.4±6.76µs     5.6 MB/sec    1.00    522.3±6.36µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.04ms     3.9 MB/sec    1.00      6.5±0.04ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.03ms     5.1 MB/sec    1.00      8.1±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1784.0±10.40µs     9.3 MB/sec    1.00  1788.2±12.21µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.8±1.65µs    14.9 MB/sec    1.02   202.6±17.81µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.8 MB/sec    1.01      3.8±0.05ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
