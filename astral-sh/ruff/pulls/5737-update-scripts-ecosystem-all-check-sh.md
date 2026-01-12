```yaml
number: 5737
title: Update scripts/ecosystem_all_check.sh
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: update_ecosystem_scripts
created_at: 2023-07-13T12:09:06Z
updated_at: 2023-07-13T13:25:24Z
url: https://github.com/astral-sh/ruff/pull/5737
synced_at: 2026-01-12T15:55:19Z
```

# Update scripts/ecosystem_all_check.sh

---

_@konstin_

## Summary

These changes make `scripts/ecosystem_all_check.sh --select ALL` work again, i forgot to update this script to the new directory structure from #5299 because it's only run manually


## Test Plan

n/a

---

_Label `internal` added by @konstin on 2023-07-13 12:09_

---

_Comment by @github-actions[bot] on 2023-07-13 12:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.10      8.9±0.03ms     4.6 MB/sec    1.00      8.1±0.05ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.07   1983.6±1.84µs     8.4 MB/sec    1.00   1854.6±9.55µs     9.0 MB/sec
formatter/numpy/globals.py                 1.04    216.0±0.24µs    13.7 MB/sec    1.00    207.0±0.38µs    14.3 MB/sec
formatter/pydantic/types.py                1.09      4.4±0.01ms     5.9 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.01     13.6±0.09ms     3.0 MB/sec    1.00     13.5±0.03ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    451.5±0.58µs     6.5 MB/sec    1.00    452.0±3.93µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1467.7±6.53µs    11.3 MB/sec    1.00   1473.8±7.65µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.7±0.23µs    17.7 MB/sec    1.00    167.0±0.38µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.3 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.6±0.04ms     4.3 MB/sec    1.00      9.5±0.04ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     7.8 MB/sec    1.00      2.1±0.02ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    228.5±2.07µs    12.9 MB/sec    1.02    232.4±9.68µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.02ms     5.4 MB/sec    1.00      4.7±0.03ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.01     16.1±0.07ms     2.5 MB/sec    1.00     15.9±0.08ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.03ms     3.9 MB/sec    1.00      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.7±9.96µs     6.8 MB/sec    1.00    433.2±5.86µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.04ms     3.5 MB/sec    1.00      7.2±0.17ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.17ms     4.9 MB/sec    1.00      8.2±0.03ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1685.1±12.71µs     9.9 MB/sec    1.00  1682.8±11.02µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.2±1.32µs    16.2 MB/sec    1.00    182.3±1.95µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.9 MB/sec    1.00      3.7±0.02ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-13 13:08_

Can you explain what you mean by *forgot earlier*. What change broke the script, or why is the path now different?

---

_Comment by @konstin on 2023-07-13 13:25_

done

---

_Merged by @konstin on 2023-07-13 13:25_

---

_Closed by @konstin on 2023-07-13 13:25_

---

_Branch deleted on 2023-07-13 13:25_

---
