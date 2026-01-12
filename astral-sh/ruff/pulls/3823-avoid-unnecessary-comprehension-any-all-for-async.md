```yaml
number: 3823
title: "Avoid `unnecessary-comprehension-any-all` for async generators"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/async
created_at: 2023-03-30T18:33:02Z
updated_at: 2023-03-30T18:59:11Z
url: https://github.com/astral-sh/ruff/pull/3823
synced_at: 2026-01-12T04:28:19Z
```

# Avoid `unnecessary-comprehension-any-all` for async generators

---

_Pull request opened by @charliermarsh on 2023-03-30 18:33_

Closes #3818.

---

_@charliermarsh reviewed on 2023-03-30 18:33_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_pie/PIE802.py`:11 on 2023-03-30 18:33_

(Inverted the order of this fixture: we typically put errors up-top, and non-errors on bottom.)

---

_Merged by @charliermarsh on 2023-03-30 18:44_

---

_Closed by @charliermarsh on 2023-03-30 18:44_

---

_Branch deleted on 2023-03-30 18:44_

---

_Comment by @github-actions[bot] on 2023-03-30 18:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.6±0.62ms     2.3 MB/sec    1.06     18.7±0.89ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.6±0.26ms     3.7 MB/sec    1.00      4.5±0.26ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   580.9±38.35µs     5.1 MB/sec    1.01   587.4±28.47µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.24ms     3.4 MB/sec    1.04      7.7±0.26ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.21ms     4.7 MB/sec    1.11      9.7±0.35ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1995.4±94.52µs     8.3 MB/sec    1.05      2.1±0.08ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   234.7±10.59µs    12.6 MB/sec    1.06   249.1±16.46µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.17ms     6.3 MB/sec    1.10      4.5±0.24ms     5.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.1±0.09ms     3.1 MB/sec    1.00     13.0±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    350.2±2.50µs     8.4 MB/sec    1.00    348.2±3.56µs     8.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.5±0.04ms     4.6 MB/sec    1.00      5.5±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.03ms     5.8 MB/sec    1.00      6.9±0.03ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1463.5±6.37µs    11.4 MB/sec    1.00  1449.2±11.49µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.02   154.7±18.70µs    19.1 MB/sec    1.00    151.6±1.84µs    19.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.02ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
