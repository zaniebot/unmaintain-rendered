```yaml
number: 4152
title: "Remove `pyright` comment prefix from PYI033 checks"
type: pull_request
state: merged
author: evanrittenhouse
labels:
  - bug
assignees: []
merged: true
base: main
head: 4151_pyi033_falsepos
created_at: 2023-04-29T19:32:06Z
updated_at: 2023-06-15T21:14:07Z
url: https://github.com/astral-sh/ruff/pull/4152
synced_at: 2026-01-12T15:55:14Z
```

# Remove `pyright` comment prefix from PYI033 checks

---

_@evanrittenhouse_

Should close #4151 by removing `pyright` prefixes from the specified regex.

---

_Marked ready for review by @evanrittenhouse on 2023-04-29 19:38_

---

_Comment by @github-actions[bot] on 2023-04-29 19:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.1±0.06ms     2.9 MB/sec    1.00     13.9±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.4±0.01ms     4.9 MB/sec    1.00      3.3±0.03ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    422.6±2.14µs     7.0 MB/sec    1.00    413.3±0.82µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      7.0±0.01ms     5.8 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1507.7±3.17µs    11.0 MB/sec    1.00   1488.3±4.25µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    169.9±0.36µs    17.4 MB/sec    1.00    168.4±0.85µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.1 MB/sec    1.00      3.1±0.02ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.5 MB/sec    1.01      5.5±0.13ms     7.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1060.3±0.54µs    15.7 MB/sec    1.00   1064.9±0.87µs    15.6 MB/sec
parser/numpy/globals.py                    1.00    108.2±0.74µs    27.3 MB/sec    1.00    108.3±0.42µs    27.2 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     20.5±0.74ms  2027.4 KB/sec    1.00     20.4±0.76ms  2039.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.17ms     3.2 MB/sec    1.00      5.2±0.22ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   614.9±30.45µs     4.8 MB/sec    1.00   607.5±32.72µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.34ms     2.9 MB/sec    1.02      8.9±0.38ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.5±0.41ms     3.9 MB/sec    1.02     10.7±0.33ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.09ms     7.5 MB/sec    1.04      2.3±0.08ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   261.2±12.31µs    11.3 MB/sec    1.02   265.7±15.26µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.19ms     5.4 MB/sec    1.03      4.9±0.17ms     5.3 MB/sec
parser/large/dataset.py                    1.02      8.7±0.22ms     4.7 MB/sec    1.00      8.5±0.24ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.02  1698.5±93.46µs     9.8 MB/sec    1.00  1658.3±67.49µs    10.0 MB/sec
parser/numpy/globals.py                    1.00    168.8±8.95µs    17.5 MB/sec    1.00    168.3±7.98µs    17.5 MB/sec
parser/pydantic/types.py                   1.01      3.7±0.12ms     6.9 MB/sec    1.00      3.7±0.15ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-29 22:41_

---

_Closed by @charliermarsh on 2023-04-29 22:41_

---

_Label `bug` added by @charliermarsh on 2023-04-29 22:41_

---

_Branch deleted on 2023-06-15 21:14_

---
