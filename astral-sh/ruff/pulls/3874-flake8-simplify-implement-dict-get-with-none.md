```yaml
number: 3874
title: "[`flake8-simplify`] Implement `dict-get-with-none-default` (`SIM910`)"
type: pull_request
state: merged
author: kyoto7250
labels:
  - rule
assignees: []
merged: true
base: main
head: impl_SIM910
created_at: 2023-04-04T03:00:47Z
updated_at: 2023-04-04T04:05:23Z
url: https://github.com/astral-sh/ruff/pull/3874
synced_at: 2026-01-12T04:28:19Z
```

# [`flake8-simplify`] Implement `dict-get-with-none-default` (`SIM910`)

---

_Pull request opened by @kyoto7250 on 2023-04-04 03:00_

close #3546 

This PR implements `SIM910` of `flake8-simplify`.

ref:

https://github.com/MartinThoma/flake8-simplify/pull/173


---

_Comment by @github-actions[bot] on 2023-04-04 03:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.2±0.07ms     2.4 MB/sec    1.00     17.1±0.06ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.00      4.3±0.09ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    541.5±2.33µs     5.4 MB/sec    1.00    541.7±1.34µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.02ms     3.5 MB/sec    1.00      7.2±0.02ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.02ms     4.6 MB/sec    1.00      8.7±0.02ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1976.7±7.42µs     8.4 MB/sec    1.00   1957.8±5.72µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    218.1±0.41µs    13.5 MB/sec    1.00    216.3±2.08µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.8±0.68ms     2.2 MB/sec    1.01     19.0±0.57ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.18ms     3.5 MB/sec    1.01      4.8±0.24ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   568.4±23.82µs     5.2 MB/sec    1.03   586.1±32.44µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.33ms     3.2 MB/sec    1.03      8.1±0.26ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.24ms     4.2 MB/sec    1.01      9.9±0.27ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.11ms     7.8 MB/sec    1.00      2.1±0.07ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   229.5±12.55µs    12.9 MB/sec    1.00   228.7±11.81µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.5±0.16ms     5.6 MB/sec    1.00      4.4±0.16ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "impl SIM910" to "[`flake8-simplify`] Implement `dict-get-with-none-default` (`SIM910`)" by @charliermarsh on 2023-04-04 03:47_

---

_Label `rule` added by @charliermarsh on 2023-04-04 03:47_

---

_Merged by @charliermarsh on 2023-04-04 03:52_

---

_Closed by @charliermarsh on 2023-04-04 03:52_

---
