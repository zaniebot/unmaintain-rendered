```yaml
number: 5805
title: "Add documentation to the `S5XX` rules"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: s5-docs
created_at: 2023-07-16T13:04:16Z
updated_at: 2023-07-17T09:39:19Z
url: https://github.com/astral-sh/ruff/pull/5805
synced_at: 2026-01-12T15:55:19Z
```

# Add documentation to the `S5XX` rules

---

_@tjkuson_

## Summary

Add documentation to the `S5XX` rules (the `flake8-bandit` ['cryptography'](https://bandit.readthedocs.io/en/latest/plugins/index.html#plugin-id-groupings) rule group). Related to #2646.

## Test Plan

`python scripts/check_docs_formatted.py`


---

_Comment by @github-actions[bot] on 2023-07-16 13:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.02ms     4.4 MB/sec    1.00      9.3±0.02ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1866.3±5.16µs     8.9 MB/sec    1.00   1866.5±3.72µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    210.2±0.56µs    14.0 MB/sec    1.00    210.1±0.41µs    14.0 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.00ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.08     14.0±0.02ms     2.9 MB/sec    1.00     13.0±0.03ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      3.5±0.01ms     4.8 MB/sec    1.00      3.3±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    443.7±0.68µs     6.7 MB/sec    1.00    436.0±0.49µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.06      6.3±0.05ms     4.1 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.14      7.5±0.01ms     5.5 MB/sec    1.00      6.5±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.11   1553.0±3.13µs    10.7 MB/sec    1.00   1394.5±5.63µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.07    165.7±0.68µs    17.8 MB/sec    1.00    154.8±0.44µs    19.1 MB/sec
linter/default-rules/pydantic/types.py     1.12      3.3±0.01ms     7.8 MB/sec    1.00      2.9±0.01ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.2±0.35ms     3.1 MB/sec    1.01     13.3±0.40ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.7±0.10ms     6.3 MB/sec    1.00      2.6±0.11ms     6.3 MB/sec
formatter/numpy/globals.py                 1.00   299.1±13.91µs     9.9 MB/sec    1.01   303.5±24.33µs     9.7 MB/sec
formatter/pydantic/types.py                1.00      5.7±0.26ms     4.5 MB/sec    1.00      5.7±0.24ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     18.7±0.39ms     2.2 MB/sec    1.00     18.7±0.40ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.16ms     3.4 MB/sec    1.00      4.9±0.16ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   593.8±22.13µs     5.0 MB/sec    1.00   596.2±22.55µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.5±0.24ms     3.0 MB/sec    1.00      8.4±0.23ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.02      9.6±0.22ms     4.3 MB/sec    1.00      9.4±0.23ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.0±0.06ms     8.3 MB/sec    1.00  1978.0±57.29µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.02   233.0±10.23µs    12.7 MB/sec    1.00    229.5±9.21µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.3±0.16ms     5.9 MB/sec    1.00      4.3±0.16ms     6.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-07-17 01:58_

---

_Merged by @charliermarsh on 2023-07-17 02:12_

---

_Closed by @charliermarsh on 2023-07-17 02:12_

---

_Branch deleted on 2023-07-17 09:39_

---
