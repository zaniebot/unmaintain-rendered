```yaml
number: 4161
title: Fix F811 false positive with match
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - bug
assignees: []
merged: true
base: main
head: 04-30-Fix_F811_false_positive_with_match
created_at: 2023-04-30T13:08:44Z
updated_at: 2023-04-30T18:45:55Z
url: https://github.com/astral-sh/ruff/pull/4161
synced_at: 2026-01-12T15:55:14Z
```

# Fix F811 false positive with match

---

_@JonathanPlasse_

- Close #4076

---

_Comment by @github-actions[bot] on 2023-04-30 13:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.9±0.04ms     2.9 MB/sec    1.00     13.8±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    418.6±0.78µs     7.0 MB/sec    1.00    410.9±0.52µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.01ms     4.4 MB/sec    1.00      5.7±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.01ms     5.9 MB/sec    1.01      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1482.2±1.70µs    11.2 MB/sec    1.01   1501.0±4.57µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.0±0.26µs    17.7 MB/sec    1.02    169.6±0.69µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.01      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.5 MB/sec    1.01      5.4±0.00ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1057.7±0.61µs    15.7 MB/sec    1.00   1058.8±0.50µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    107.9±0.27µs    27.3 MB/sec    1.01    109.0±0.33µs    27.1 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.1±0.15ms     2.5 MB/sec    1.01     16.4±0.19ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.01      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   476.5±12.83µs     6.2 MB/sec    1.01    481.9±8.73µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.09ms     3.8 MB/sec    1.02      6.9±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.12ms     4.9 MB/sec    1.01      8.3±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1723.5±19.77µs     9.7 MB/sec    1.03  1778.5±22.49µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.1±5.13µs    15.5 MB/sec    1.07    204.2±7.60µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.07ms     7.0 MB/sec    1.04      3.8±0.06ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.9±0.07ms     5.9 MB/sec    1.00      6.8±0.05ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.01  1316.6±24.67µs    12.6 MB/sec    1.00  1310.0±22.12µs    12.7 MB/sec
parser/numpy/globals.py                    1.02    135.2±3.57µs    21.8 MB/sec    1.00    132.7±2.97µs    22.2 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.04ms     8.8 MB/sec    1.00      2.9±0.04ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-30 18:39_

---

_Closed by @charliermarsh on 2023-04-30 18:39_

---

_Label `bug` added by @charliermarsh on 2023-04-30 18:39_

---

_Branch deleted on 2023-04-30 18:45_

---
