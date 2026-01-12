```yaml
number: 3912
title: "Avoid N802 violations for `@override` methods"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/override
created_at: 2023-04-08T03:03:52Z
updated_at: 2023-04-08T03:27:02Z
url: https://github.com/astral-sh/ruff/pull/3912
synced_at: 2026-01-12T15:55:14Z
```

# Avoid N802 violations for `@override` methods

---

_@charliermarsh_

Closes #3910.

---

_Merged by @charliermarsh on 2023-04-08 03:11_

---

_Closed by @charliermarsh on 2023-04-08 03:11_

---

_Branch deleted on 2023-04-08 03:11_

---

_Comment by @github-actions[bot] on 2023-04-08 03:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.06ms     2.7 MB/sec    1.00     15.1±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.4 MB/sec    1.00      3.8±0.03ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    410.8±1.24µs     7.2 MB/sec    1.00    410.3±1.13µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.02ms     4.0 MB/sec    1.00      6.4±0.02ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.02ms     5.2 MB/sec    1.00      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1732.5±11.58µs     9.6 MB/sec    1.00   1727.7±2.64µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.5±0.28µs    16.3 MB/sec    1.00    182.1±0.49µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.29ms     2.5 MB/sec    1.00     16.2±0.26ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     4.0 MB/sec    1.00      4.2±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    493.6±8.35µs     6.0 MB/sec    1.00    495.3±7.76µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.10ms     3.7 MB/sec    1.00      6.9±0.13ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.4±0.09ms     4.9 MB/sec    1.00      8.2±0.11ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1829.9±24.04µs     9.1 MB/sec    1.01  1845.8±32.32µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.2±3.51µs    15.1 MB/sec    1.02   199.9±10.15µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.09ms     6.7 MB/sec    1.00      3.7±0.05ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
