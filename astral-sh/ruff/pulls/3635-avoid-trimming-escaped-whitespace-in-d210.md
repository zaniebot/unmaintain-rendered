```yaml
number: 3635
title: Avoid trimming escaped whitespace in D210
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: charlie/back
created_at: 2023-03-20T21:11:23Z
updated_at: 2023-03-20T21:39:26Z
url: https://github.com/astral-sh/ruff/pull/3635
synced_at: 2026-01-12T15:55:13Z
```

# Avoid trimming escaped whitespace in D210

---

_@charliermarsh_

Closes #3618.

---

_Label `bug` added by @charliermarsh on 2023-03-20 21:11_

---

_Label `docstring` added by @charliermarsh on 2023-03-20 21:11_

---

_Merged by @charliermarsh on 2023-03-20 21:17_

---

_Closed by @charliermarsh on 2023-03-20 21:17_

---

_Branch deleted on 2023-03-20 21:17_

---

_Comment by @github-actions[bot] on 2023-03-20 21:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.05ms     2.9 MB/sec    1.01     14.2±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.03ms     4.5 MB/sec    1.00      3.7±0.00ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.8±1.39µs     6.9 MB/sec    1.00    426.9±1.48µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.3±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.03ms     5.3 MB/sec    1.01      7.8±0.02ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1691.9±3.89µs     9.8 MB/sec    1.01   1703.1±2.67µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.2±0.38µs    16.8 MB/sec    1.01    176.1±0.49µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.02      3.7±0.02ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.3±0.82ms     2.3 MB/sec    1.06     18.4±0.65ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.22ms     3.5 MB/sec    1.04      5.0±0.18ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   615.7±36.18µs     4.8 MB/sec    1.05   647.1±25.04µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.29ms     3.3 MB/sec    1.08      8.4±0.39ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.4±0.48ms     4.3 MB/sec    1.11     10.5±0.29ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     7.9 MB/sec    1.13      2.4±0.14ms     6.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   257.0±13.62µs    11.5 MB/sec    1.05   270.5±18.18µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.21ms     5.6 MB/sec    1.08      4.9±0.43ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
