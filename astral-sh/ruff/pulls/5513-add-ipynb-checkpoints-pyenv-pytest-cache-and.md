```yaml
number: 5513
title: Add .ipynb_checkpoints, .pyenv, .pytest_cache, and .vscode to default excludes
type: pull_request
state: merged
author: charliermarsh
labels:
  - breaking
  - cli
assignees: []
merged: true
base: main
head: charlie/exclusions
created_at: 2023-07-04T20:11:39Z
updated_at: 2023-07-04T20:46:29Z
url: https://github.com/astral-sh/ruff/pull/5513
synced_at: 2026-01-12T03:36:55Z
```

# Add .ipynb_checkpoints, .pyenv, .pytest_cache, and .vscode to default excludes

---

_Pull request opened by @charliermarsh on 2023-07-04 20:11_

## Summary

VS Code extensions are [recommended](https://code.visualstudio.com/docs/python/settings-reference#_linting-settings) to exclude `.vscode` and `site-packages`. Black also now omits `.vscode`, `.pytest_cache`, and `.ipynb_checkpoints` by default. Omitting `.pyenv` is similar to omitting virtual environments, but really only matters in the context of VS Code (see: https://github.com/astral-sh/ruff/discussions/5509).

Closes: #5510.


---

_Label `breaking` added by @charliermarsh on 2023-07-04 20:11_

---

_Label `cli` added by @charliermarsh on 2023-07-04 20:11_

---

_Merged by @charliermarsh on 2023-07-04 20:25_

---

_Closed by @charliermarsh on 2023-07-04 20:25_

---

_Branch deleted on 2023-07-04 20:25_

---

_Comment by @github-actions[bot] on 2023-07-04 20:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.02ms     4.9 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1776.1±3.83µs     9.4 MB/sec    1.00   1774.5±4.67µs     9.4 MB/sec
formatter/numpy/globals.py                 1.00    195.9±0.27µs    15.1 MB/sec    1.00    196.3±0.59µs    15.0 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.4 MB/sec    1.00      3.9±0.01ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.01     14.2±0.04ms     2.9 MB/sec    1.00     14.0±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    367.3±0.81µs     8.0 MB/sec    1.00    366.1±1.13µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.01ms     4.1 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.01ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1485.0±1.91µs    11.2 MB/sec    1.00   1470.6±3.55µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    159.7±0.23µs    18.5 MB/sec    1.00    158.4±0.41µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.11ms     4.4 MB/sec    1.00      9.3±0.10ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.03ms     8.2 MB/sec    1.01      2.0±0.07ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    231.3±4.85µs    12.8 MB/sec    1.04   240.3±24.92µs    12.3 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.09ms     5.7 MB/sec    1.01      4.5±0.07ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.14ms     2.6 MB/sec    1.01     15.6±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.07ms     4.1 MB/sec    1.00      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.5±7.02µs     5.9 MB/sec    1.00   499.3±10.52µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.12ms     3.7 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.07ms     5.1 MB/sec    1.00      8.0±0.10ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1710.1±38.37µs     9.7 MB/sec    1.00  1716.4±42.37µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.4±5.14µs    14.7 MB/sec    1.00    200.8±4.81µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.1 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
