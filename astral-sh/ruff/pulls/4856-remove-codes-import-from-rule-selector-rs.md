```yaml
number: 4856
title: Remove codes import from rule_selector.rs
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/nurse
created_at: 2023-06-05T02:25:40Z
updated_at: 2023-06-05T02:55:12Z
url: https://github.com/astral-sh/ruff/pull/4856
synced_at: 2026-01-12T15:55:16Z
```

# Remove codes import from rule_selector.rs

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-05 02:27_

---

_Merged by @charliermarsh on 2023-06-05 02:31_

---

_Closed by @charliermarsh on 2023-06-05 02:31_

---

_Branch deleted on 2023-06-05 02:31_

---

_Comment by @github-actions[bot] on 2023-06-05 02:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.8±0.04ms     2.9 MB/sec    1.00     13.8±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    412.4±1.55µs     7.2 MB/sec    1.00    413.7±0.77µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.05ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.01ms     6.1 MB/sec    1.00      6.6±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1440.4±7.12µs    11.6 MB/sec    1.00   1445.8±2.06µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.06    168.0±5.70µs    17.6 MB/sec    1.00    159.2±1.59µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.8 MB/sec    1.01      5.2±0.00ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1018.6±0.81µs    16.3 MB/sec    1.01   1024.3±1.02µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    106.6±0.19µs    27.7 MB/sec    1.01    107.4±0.48µs    27.5 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.01ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.15ms     2.4 MB/sec    1.00     16.7±0.21ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    496.0±8.47µs     5.9 MB/sec    1.00    490.3±6.52µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.07ms     3.6 MB/sec    1.00      7.0±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.09ms     4.9 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1750.8±19.62µs     9.5 MB/sec    1.00  1751.1±28.75µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.5±3.60µs    14.9 MB/sec    1.01    199.1±6.41µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.01      3.8±0.07ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.5±0.04ms     6.3 MB/sec    1.00      6.5±0.04ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1230.0±14.79µs    13.5 MB/sec    1.00  1230.2±14.29µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    127.0±2.22µs    23.2 MB/sec    1.00    126.5±2.36µs    23.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.2 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
