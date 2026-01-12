```yaml
number: 4084
title: "Add `PrefectHQ/prefect` to list of ruff users"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: using-prefect
created_at: 2023-04-24T19:49:41Z
updated_at: 2023-04-24T23:49:12Z
url: https://github.com/astral-sh/ruff/pull/4084
synced_at: 2026-01-12T04:28:19Z
```

# Add `PrefectHQ/prefect` to list of ruff users

---

_Pull request opened by @zanieb on 2023-04-24 19:49_

Prefect switched their main repository to use ruff in https://github.com/PrefectHQ/prefect/pull/9283

Retains the reference to the `PrefectHQ/marvin` project following the style of other organizations with multiple projects.

Preview at https://github.com/madkinsz/ruff/blob/using-prefect/README.md#whos-using-ruff


---

_Comment by @github-actions[bot] on 2023-04-24 20:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.3±0.09ms     2.3 MB/sec    1.00     17.4±0.09ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.01      4.3±0.02ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    554.1±2.23µs     5.3 MB/sec    1.00    554.1±1.06µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.03ms     3.5 MB/sec    1.01      7.3±0.01ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.03ms     4.6 MB/sec    1.00      8.9±0.03ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1982.2±5.25µs     8.4 MB/sec    1.01   1999.4±3.92µs     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    220.3±1.17µs    13.4 MB/sec    1.00    221.3±7.66µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.01ms     6.3 MB/sec    1.00      4.1±0.02ms     6.2 MB/sec
parser/large/dataset.py                    1.00      7.0±0.02ms     5.8 MB/sec    1.00      7.0±0.02ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1378.6±4.41µs    12.1 MB/sec    1.00   1377.8±5.15µs    12.1 MB/sec
parser/numpy/globals.py                    1.00    135.5±0.73µs    21.8 MB/sec    1.01    136.3±0.44µs    21.6 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.00ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.07ms     2.4 MB/sec    1.00     16.9±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.02ms     3.7 MB/sec    1.00      4.5±0.02ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    476.7±3.93µs     6.2 MB/sec    1.01    479.2±5.93µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.03ms     3.5 MB/sec    1.00      7.3±0.02ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.04ms     4.6 MB/sec    1.00      8.8±0.05ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1905.3±6.64µs     8.7 MB/sec    1.01  1921.1±50.85µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.8±1.59µs    14.8 MB/sec    1.00    199.9±2.05µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.02ms     6.3 MB/sec    1.00      4.0±0.04ms     6.3 MB/sec
parser/large/dataset.py                    1.00      6.9±0.02ms     5.9 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1319.3±4.21µs    12.6 MB/sec    1.00  1320.5±10.78µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    133.5±0.96µs    22.1 MB/sec    1.00    133.5±0.79µs    22.1 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.02ms     8.6 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-04-24 23:49_

Amazing, thanks @madkinsz!

---

_Merged by @charliermarsh on 2023-04-24 23:49_

---

_Closed by @charliermarsh on 2023-04-24 23:49_

---
