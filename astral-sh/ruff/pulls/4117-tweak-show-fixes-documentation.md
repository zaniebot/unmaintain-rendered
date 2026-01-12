```yaml
number: 4117
title: "Tweak `--show-fixes` documentation"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/docs-fix
created_at: 2023-04-26T15:09:31Z
updated_at: 2023-04-26T17:18:02Z
url: https://github.com/astral-sh/ruff/pull/4117
synced_at: 2026-01-12T15:55:14Z
```

# Tweak `--show-fixes` documentation

---

_@charliermarsh_

Closes #4112.

---

_Merged by @charliermarsh on 2023-04-26 15:15_

---

_Closed by @charliermarsh on 2023-04-26 15:15_

---

_Branch deleted on 2023-04-26 15:15_

---

_Comment by @github-actions[bot] on 2023-04-26 15:19_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     17.0±0.21ms     2.4 MB/sec    1.00     16.7±0.37ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.3±0.05ms     3.9 MB/sec    1.00      4.1±0.12ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.05   547.2±13.15µs     5.4 MB/sec    1.00   521.9±16.52µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.14ms     3.6 MB/sec    1.00      6.9±0.17ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.28ms     4.9 MB/sec    1.01      8.5±0.20ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1886.3±54.19µs     8.8 MB/sec    1.03  1935.3±34.61µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.3±7.37µs    14.5 MB/sec    1.04    211.5±6.01µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.08ms     6.8 MB/sec    1.05      3.9±0.08ms     6.5 MB/sec
parser/large/dataset.py                    1.01      6.7±0.18ms     6.1 MB/sec    1.00      6.6±0.17ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1330.2±27.65µs    12.5 MB/sec    1.00  1325.0±35.98µs    12.6 MB/sec
parser/numpy/globals.py                    1.02    134.8±2.56µs    21.9 MB/sec    1.00    131.9±3.31µs    22.4 MB/sec
parser/pydantic/types.py                   1.02      2.9±0.07ms     8.7 MB/sec    1.00      2.9±0.07ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.11ms     2.4 MB/sec    1.00     17.1±0.10ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.05ms     3.7 MB/sec    1.00      4.5±0.02ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    481.4±9.49µs     6.1 MB/sec    1.00    479.4±5.51µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.05ms     3.5 MB/sec    1.00      7.4±0.35ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.05ms     4.6 MB/sec    1.00      8.9±0.06ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1932.2±17.83µs     8.6 MB/sec    1.00  1935.5±15.64µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.1±5.93µs    14.7 MB/sec    1.01    203.3±5.93µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.03ms     6.3 MB/sec    1.00      4.1±0.04ms     6.3 MB/sec
parser/large/dataset.py                    1.00      6.9±0.04ms     5.9 MB/sec    1.01      7.0±0.08ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1325.9±20.39µs    12.6 MB/sec    1.01  1337.8±39.97µs    12.4 MB/sec
parser/numpy/globals.py                    1.00    134.5±1.24µs    21.9 MB/sec    1.00    134.8±1.00µs    21.9 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.02ms     8.6 MB/sec    1.01      3.0±0.04ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @jamesbraza on 2023-04-26 17:16_

One comment for posterity is this closed https://github.com/charliermarsh/ruff/issues/4113, not https://github.com/charliermarsh/ruff/issues/4112.

And thank you for the quick turnaround @charliermarsh!

---

_Comment by @charliermarsh on 2023-04-26 17:18_

Thank you, I messed up the reference :joy:

---
