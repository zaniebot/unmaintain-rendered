```yaml
number: 3920
title: "Remove `extract_path_names` helper"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/file-name
created_at: 2023-04-08T14:46:05Z
updated_at: 2023-04-08T15:14:43Z
url: https://github.com/astral-sh/ruff/pull/3920
synced_at: 2026-01-12T04:28:19Z
```

# Remove `extract_path_names` helper

---

_Pull request opened by @charliermarsh on 2023-04-08 14:46_

It turns out that we can pass any `AsRef<Path>` to `globset::GlobSet::is_match`, which lets us simplify a lot of the logic here.

---

_Label `internal` added by @charliermarsh on 2023-04-08 14:46_

---

_Comment by @github-actions[bot] on 2023-04-08 14:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     15.3±0.12ms     2.7 MB/sec    1.00     15.0±0.01ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.00ms     4.3 MB/sec    1.00      3.8±0.00ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    415.2±1.88µs     7.1 MB/sec    1.00    410.8±1.58µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.5±0.01ms     3.9 MB/sec    1.00      6.4±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.03      8.0±0.01ms     5.1 MB/sec    1.00      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1749.6±2.07µs     9.5 MB/sec    1.00   1720.9±4.06µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.03    182.8±0.33µs    16.1 MB/sec    1.00    178.1±0.54µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.01ms     6.9 MB/sec    1.00      3.6±0.00ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     16.7±0.21ms     2.4 MB/sec    1.00     16.3±0.21ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.08ms     4.0 MB/sec    1.00      4.1±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   498.7±10.86µs     5.9 MB/sec    1.00   496.8±11.40µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.2±0.17ms     3.6 MB/sec    1.00      6.9±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.13ms     4.9 MB/sec    1.01      8.3±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1818.5±24.86µs     9.2 MB/sec    1.00  1806.0±22.71µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.6±3.85µs    15.3 MB/sec    1.04    200.6±7.86µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.02      3.8±0.06ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-08 15:14_

---

_Closed by @charliermarsh on 2023-04-08 15:14_

---

_Branch deleted on 2023-04-08 15:14_

---
