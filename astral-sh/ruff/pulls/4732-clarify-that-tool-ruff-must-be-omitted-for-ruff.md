```yaml
number: 4732
title: "Clarify that [tool.ruff] must be omitted for ruff.toml"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/ruff.toml
created_at: 2023-05-30T17:27:07Z
updated_at: 2023-05-30T17:54:07Z
url: https://github.com/astral-sh/ruff/pull/4732
synced_at: 2026-01-12T03:50:03Z
```

# Clarify that [tool.ruff] must be omitted for ruff.toml

---

_Pull request opened by @charliermarsh on 2023-05-30 17:27_

Closes #4725.

---

_Label `documentation` added by @charliermarsh on 2023-05-30 17:27_

---

_Merged by @charliermarsh on 2023-05-30 17:35_

---

_Closed by @charliermarsh on 2023-05-30 17:35_

---

_Branch deleted on 2023-05-30 17:35_

---

_Comment by @github-actions[bot] on 2023-05-30 17:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.17ms     2.5 MB/sec    1.01     16.3±0.29ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.05ms     4.2 MB/sec    1.00      3.9±0.06ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    494.0±8.04µs     6.0 MB/sec    1.00    491.3±8.36µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.13ms     3.7 MB/sec    1.00      6.8±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.12ms     5.2 MB/sec    1.00      7.9±0.10ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1750.9±30.13µs     9.5 MB/sec    1.01  1765.9±25.68µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    200.8±3.28µs    14.7 MB/sec    1.00    197.8±4.65µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.0±0.08ms     6.8 MB/sec    1.00      6.0±0.09ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1192.7±17.80µs    14.0 MB/sec    1.00  1187.9±25.14µs    14.0 MB/sec
parser/numpy/globals.py                    1.00    122.1±2.66µs    24.2 MB/sec    1.00    122.0±2.56µs    24.2 MB/sec
parser/pydantic/types.py                   1.01      2.6±0.04ms     9.7 MB/sec    1.00      2.6±0.03ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.08ms     2.7 MB/sec    1.00     15.1±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    391.0±2.01µs     7.5 MB/sec    1.00    392.4±2.98µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.00      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.03ms     5.3 MB/sec    1.00      7.6±0.04ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1555.9±7.86µs    10.7 MB/sec    1.01   1563.7±6.37µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.4±1.60µs    16.9 MB/sec    1.00    173.8±1.44µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.02ms     7.6 MB/sec    1.00      3.3±0.02ms     7.6 MB/sec
parser/large/dataset.py                    1.06      6.6±0.02ms     6.2 MB/sec    1.00      6.2±0.03ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.05   1221.2±5.12µs    13.6 MB/sec    1.00   1158.8±5.59µs    14.4 MB/sec
parser/numpy/globals.py                    1.04    123.9±0.81µs    23.8 MB/sec    1.00    118.7±0.45µs    24.9 MB/sec
parser/pydantic/types.py                   1.06      2.7±0.01ms     9.3 MB/sec    1.00      2.6±0.01ms     9.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
