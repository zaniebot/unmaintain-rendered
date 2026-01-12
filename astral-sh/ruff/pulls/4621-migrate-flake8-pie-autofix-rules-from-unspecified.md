```yaml
number: 4621
title: "Migrate flake8_pie autofix rules from `unspecified` to `suggested` and `automatic`"
type: pull_request
state: merged
author: qdegraaf
labels: []
assignees: []
merged: true
base: main
head: fix/migrateflake8pie
created_at: 2023-05-24T08:32:33Z
updated_at: 2023-05-24T17:03:38Z
url: https://github.com/astral-sh/ruff/pull/4621
synced_at: 2026-01-12T03:50:03Z
```

# Migrate flake8_pie autofix rules from `unspecified` to `suggested` and `automatic`

---

_Pull request opened by @qdegraaf on 2023-05-24 08:32_

## Summary
Relates to: https://github.com/charliermarsh/ruff/issues/4184 

Might be a bit optimistic with PIE790 and PIE807 but the fix code looks precise to me. Can change if I'm missing something



---

_Comment by @github-actions[bot] on 2023-05-24 08:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.08ms     2.4 MB/sec    1.00     16.9±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.01ms     4.1 MB/sec    1.00      4.0±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.02    510.3±1.40µs     5.8 MB/sec    1.00    499.1±1.22µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.04ms     3.6 MB/sec    1.00      7.0±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.03ms     5.0 MB/sec    1.00      8.2±0.03ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1817.1±7.49µs     9.2 MB/sec    1.00   1803.3±4.63µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.08   219.9±14.44µs    13.4 MB/sec    1.00    203.2±1.67µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.01ms     6.8 MB/sec    1.00      3.7±0.01ms     6.8 MB/sec
parser/large/dataset.py                    1.02      6.3±0.01ms     6.5 MB/sec    1.00      6.2±0.01ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.01   1239.8±1.23µs    13.4 MB/sec    1.00   1228.2±8.46µs    13.6 MB/sec
parser/numpy/globals.py                    1.01    126.9±0.19µs    23.3 MB/sec    1.00    126.0±0.84µs    23.4 MB/sec
parser/pydantic/types.py                   1.01      2.7±0.00ms     9.4 MB/sec    1.00      2.7±0.00ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.7±0.41ms     2.2 MB/sec    1.05     19.7±0.82ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.12ms     3.5 MB/sec    1.04      5.0±0.23ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   573.5±17.38µs     5.1 MB/sec    1.11   637.7±56.05µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.17ms     3.2 MB/sec    1.11      8.8±0.45ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.1±0.26ms     4.5 MB/sec    1.04      9.4±0.38ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1918.3±38.54µs     8.7 MB/sec    1.09      2.1±0.19ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    234.8±8.83µs    12.6 MB/sec    1.06   248.5±15.35µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.15ms     6.3 MB/sec    1.05      4.3±0.20ms     5.9 MB/sec
parser/large/dataset.py                    1.00      7.2±0.13ms     5.6 MB/sec    1.02      7.4±0.21ms     5.5 MB/sec
parser/numpy/ctypeslib.py                  1.00  1408.1±49.22µs    11.8 MB/sec    1.01  1415.5±46.45µs    11.8 MB/sec
parser/numpy/globals.py                    1.00    142.6±4.45µs    20.7 MB/sec    1.01    144.4±6.37µs    20.4 MB/sec
parser/pydantic/types.py                   1.00      3.2±0.15ms     8.1 MB/sec    1.01      3.2±0.09ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @MichaReiser on 2023-05-24 12:26_

---

_Merged by @charliermarsh on 2023-05-24 16:08_

---

_Closed by @charliermarsh on 2023-05-24 16:08_

---

_Branch deleted on 2023-05-24 17:03_

---
