```yaml
number: 3748
title: Sort statistics by count
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: sort-statistics
created_at: 2023-03-26T20:24:12Z
updated_at: 2023-03-26T21:01:17Z
url: https://github.com/astral-sh/ruff/pull/3748
synced_at: 2026-01-12T15:55:13Z
```

# Sort statistics by count

---

_@JonathanPlasse_

- Close #3746

---

_@charliermarsh reviewed on 2023-03-26 20:24_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/printer.rs`:491 on 2023-03-26 20:24_

I think you can do `Reverse(statistic.count)`  with `use std::cmp::Reverse;`

---

_Comment by @github-actions[bot] on 2023-03-26 20:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.05     14.8±0.02ms     2.7 MB/sec    1.00     14.2±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.8±0.01ms     4.3 MB/sec    1.00      3.7±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    438.8±1.22µs     6.7 MB/sec    1.00    432.5±1.72µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.5±0.01ms     3.9 MB/sec    1.00      6.3±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.08      8.3±0.01ms     4.9 MB/sec    1.00      7.7±0.01ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06   1772.3±4.23µs     9.4 MB/sec    1.00   1672.9±1.83µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.03    181.4±0.55µs    16.3 MB/sec    1.00    176.7±0.49µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.8±0.01ms     6.7 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.09ms     2.7 MB/sec    1.02     15.3±0.18ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.01      4.1±0.07ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    527.2±7.76µs     5.6 MB/sec    1.00    529.3±8.52µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.05ms     3.9 MB/sec    1.02      6.8±0.13ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.12ms     5.0 MB/sec    1.01      8.3±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1809.2±12.67µs     9.2 MB/sec    1.01  1820.0±21.14µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.5±5.54µs    14.6 MB/sec    1.01    204.0±6.58µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.7 MB/sec    1.01      3.9±0.06ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-26 20:45_

---

_Closed by @charliermarsh on 2023-03-26 20:45_

---

_Branch deleted on 2023-03-26 21:01_

---
