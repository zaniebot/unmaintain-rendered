```yaml
number: 3729
title: "[`flake8-pyi`] Add autofix for `PYI014`"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - rule
assignees: []
merged: true
base: main
head: add-pyi014-autofix
created_at: 2023-03-25T12:50:24Z
updated_at: 2023-03-25T16:01:19Z
url: https://github.com/astral-sh/ruff/pull/3729
synced_at: 2026-01-12T15:55:13Z
```

# [`flake8-pyi`] Add autofix for `PYI014`

---

_@JonathanPlasse_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-03-25 13:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.9±0.69ms     2.3 MB/sec    1.00     18.0±0.65ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.14ms     3.7 MB/sec    1.01      4.6±0.18ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   622.4±18.07µs     4.7 MB/sec    1.02   632.1±22.92µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.25ms     3.3 MB/sec    1.08      8.4±0.30ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.28ms     4.4 MB/sec    1.02      9.5±0.28ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.08ms     8.0 MB/sec    1.02      2.1±0.08ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   245.6±10.45µs    12.0 MB/sec    1.03    253.9±8.58µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.14ms     5.7 MB/sec    1.03      4.6±0.13ms     5.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.37ms     2.4 MB/sec    1.01     16.9±0.55ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.15ms     3.8 MB/sec    1.02      4.5±0.17ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   565.2±21.10µs     5.2 MB/sec    1.04   589.2±29.79µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.25ms     3.5 MB/sec    1.02      7.5±0.29ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.24ms     4.7 MB/sec    1.02      8.9±0.32ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1878.6±67.58µs     8.9 MB/sec    1.02  1919.3±90.03µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.0±7.48µs    14.4 MB/sec    1.03   211.3±14.55µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.14ms     6.4 MB/sec    1.01      4.0±0.26ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-03-25 15:34_

---

_Renamed from "Add PYI014 auto-fix" to "[`flake8-pyi`] Add autofix for `PYI014`" by @charliermarsh on 2023-03-25 15:34_

---

_Merged by @charliermarsh on 2023-03-25 15:41_

---

_Closed by @charliermarsh on 2023-03-25 15:41_

---

_Branch deleted on 2023-03-25 15:54_

---
