```yaml
number: 4040
title: "[`flake8-import-conventions`] Implement new rule `ICN003` to ban `from ... import ...` for selected modules"
type: pull_request
state: merged
author: edgarrmondragon
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/ban-import-from
created_at: 2023-04-20T06:23:31Z
updated_at: 2023-04-23T14:16:45Z
url: https://github.com/astral-sh/ruff/pull/4040
synced_at: 2026-01-12T04:28:19Z
```

# [`flake8-import-conventions`] Implement new rule `ICN003` to ban `from ... import ...` for selected modules

---

_Pull request opened by @edgarrmondragon on 2023-04-20 06:23_

Closes https://github.com/charliermarsh/ruff/issues/4034

---

_Comment by @github-actions[bot] on 2023-04-20 06:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.6±0.02ms     2.6 MB/sec    1.00     15.5±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.2 MB/sec    1.00      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    424.7±1.39µs     6.9 MB/sec    1.00    421.3±1.31µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.01ms     3.8 MB/sec    1.00      6.7±0.02ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.04ms     5.1 MB/sec    1.01      8.0±0.02ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1758.3±7.91µs     9.5 MB/sec    1.00   1740.4±4.69µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.5±0.52µs    16.2 MB/sec    1.01    183.9±0.51µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.00      3.7±0.02ms     6.9 MB/sec
parser/large/dataset.py                    1.13      7.1±0.02ms     5.7 MB/sec    1.00      6.3±0.09ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.10   1355.7±0.85µs    12.3 MB/sec    1.00   1238.1±1.56µs    13.4 MB/sec
parser/numpy/globals.py                    1.06    134.1±0.92µs    22.0 MB/sec    1.00    126.3±0.32µs    23.4 MB/sec
parser/pydantic/types.py                   1.10      3.0±0.00ms     8.5 MB/sec    1.00      2.7±0.00ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.05     16.8±0.21ms     2.4 MB/sec    1.00     15.9±0.18ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.07      4.4±0.05ms     3.8 MB/sec    1.00      4.1±0.11ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.10   540.2±10.80µs     5.5 MB/sec    1.00    490.7±5.63µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.06      7.2±0.08ms     3.6 MB/sec    1.00      6.8±0.08ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.07      8.7±0.08ms     4.7 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1904.3±25.22µs     8.7 MB/sec    1.00  1783.1±21.03µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.11    216.8±4.84µs    13.6 MB/sec    1.00    194.7±4.79µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.07      4.0±0.04ms     6.4 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
parser/large/dataset.py                    1.04      6.9±0.04ms     5.9 MB/sec    1.00      6.6±0.04ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.04  1308.5±12.29µs    12.7 MB/sec    1.00  1254.8±17.66µs    13.3 MB/sec
parser/numpy/globals.py                    1.04    131.6±2.14µs    22.4 MB/sec    1.00    126.3±2.92µs    23.4 MB/sec
parser/pydantic/types.py                   1.05      2.9±0.03ms     8.7 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Implement new rule `ICN003` to ban `from ... import ...` for selected modules" to "[`flake8-import-conventions`] Implement new rule `ICN003` to ban `from ... import ...` for selected modules" by @edgarrmondragon on 2023-04-20 06:51_

---

_Marked ready for review by @edgarrmondragon on 2023-04-20 17:41_

---

_@MichaReiser approved on 2023-04-23 04:19_

---

_Label `rule` added by @charliermarsh on 2023-04-23 04:33_

---

_Merged by @charliermarsh on 2023-04-23 04:40_

---

_Closed by @charliermarsh on 2023-04-23 04:40_

---

_Branch deleted on 2023-04-23 14:16_

---
