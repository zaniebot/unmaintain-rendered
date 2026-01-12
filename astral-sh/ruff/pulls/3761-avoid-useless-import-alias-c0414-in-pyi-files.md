```yaml
number: 3761
title: "Avoid `useless-import alias` (`C0414`) in `.pyi` files"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/plc
created_at: 2023-03-27T18:17:34Z
updated_at: 2023-03-27T18:47:07Z
url: https://github.com/astral-sh/ruff/pull/3761
synced_at: 2026-01-12T04:39:45Z
```

# Avoid `useless-import alias` (`C0414`) in `.pyi` files

---

_Pull request opened by @charliermarsh on 2023-03-27 18:17_

Closes https://github.com/charliermarsh/ruff/issues/3734.

---

_Merged by @charliermarsh on 2023-03-27 18:27_

---

_Closed by @charliermarsh on 2023-03-27 18:27_

---

_Branch deleted on 2023-03-27 18:27_

---

_Comment by @github-actions[bot] on 2023-03-27 18:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.3±0.19ms     2.8 MB/sec    1.00     14.3±0.18ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.06ms     4.4 MB/sec    1.00      3.8±0.07ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.5±1.41µs     6.8 MB/sec    1.01    436.8±1.11µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.18ms     4.0 MB/sec    1.02      6.5±0.35ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.10ms     5.3 MB/sec    1.00      7.7±0.07ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1718.4±50.71µs     9.7 MB/sec    1.00   1688.9±1.56µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.0±0.76µs    16.7 MB/sec    1.01    178.9±1.01µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.5±0.19ms     2.6 MB/sec    1.00     15.4±0.18ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.08ms     4.0 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    533.9±7.83µs     5.5 MB/sec    1.00    530.0±5.06µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.10ms     3.7 MB/sec    1.00      6.8±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.08ms     4.9 MB/sec    1.00      8.3±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1805.2±18.94µs     9.2 MB/sec    1.01  1831.8±22.00µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.9±4.64µs    14.9 MB/sec    1.02    202.8±7.54µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.6 MB/sec    1.01      3.9±0.04ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
