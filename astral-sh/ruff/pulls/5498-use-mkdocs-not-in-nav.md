```yaml
number: 5498
title: "Use MkDocs' `not_in_nav`"
type: pull_request
state: merged
author: MicaelJarniac
labels:
  - documentation
assignees: []
merged: true
base: main
head: not_in_nav
created_at: 2023-07-04T01:27:37Z
updated_at: 2023-09-19T12:29:56Z
url: https://github.com/astral-sh/ruff/pull/5498
synced_at: 2026-01-12T02:39:09Z
```

# Use MkDocs' `not_in_nav`

---

_Pull request opened by @MicaelJarniac on 2023-07-04 01:27_

Closes #5497
Needs MkDocs 1.5 to be released.
- [x] https://github.com/mkdocs/mkdocs/milestone/15

## Summary
Uses MkDocs' `not_in_nav` config to hide spam about files in `docs/rules/` not being in nav.


---

_Comment by @github-actions[bot] on 2023-07-04 01:54_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.5±0.34ms     3.9 MB/sec    1.01     10.6±0.37ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.07ms     8.1 MB/sec    1.02      2.1±0.13ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00   241.4±10.79µs    12.2 MB/sec    1.04   251.6±21.45µs    11.7 MB/sec
formatter/pydantic/types.py                1.01      4.6±0.16ms     5.5 MB/sec    1.00      4.6±0.21ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.49ms     2.6 MB/sec    1.00     15.9±0.59ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.16ms     4.2 MB/sec    1.03      4.1±0.14ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   524.9±26.59µs     5.6 MB/sec    1.06   554.9±26.06µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.24ms     3.6 MB/sec    1.03      7.3±0.29ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.23ms     5.3 MB/sec    1.02      7.8±0.20ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1685.0±71.24µs     9.9 MB/sec    1.01  1708.8±49.97µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.02   207.6±10.25µs    14.2 MB/sec    1.00   204.3±10.91µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.13ms     7.3 MB/sec    1.00      3.5±0.12ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.3±0.63ms     3.1 MB/sec    1.00     13.3±0.76ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.13ms     6.8 MB/sec    1.02      2.5±0.14ms     6.7 MB/sec
formatter/numpy/globals.py                 1.00   281.1±17.63µs    10.5 MB/sec    1.03   290.4±16.72µs    10.2 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.24ms     4.8 MB/sec    1.05      5.5±0.29ms     4.6 MB/sec
linter/all-rules/large/dataset.py          1.00     18.7±1.01ms     2.2 MB/sec    1.03     19.3±0.98ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.0±0.22ms     3.3 MB/sec    1.00      4.8±0.17ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.03   620.3±21.32µs     4.8 MB/sec    1.00   603.4±31.44µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.11      9.2±0.33ms     2.8 MB/sec    1.00      8.2±0.40ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.07      9.9±0.23ms     4.1 MB/sec    1.00      9.2±0.47ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06      2.1±0.05ms     8.0 MB/sec    1.00  1975.8±108.14µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    242.8±8.93µs    12.2 MB/sec    1.00   241.0±15.92µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.28ms     6.0 MB/sec    1.01      4.3±0.25ms     6.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-09-18 21:14_

MkDocs 1.5 has been released.

---

_Label `documentation` added by @charliermarsh on 2023-09-18 23:51_

---

_Marked ready for review by @charliermarsh on 2023-09-18 23:51_

---

_Comment by @charliermarsh on 2023-09-18 23:52_

Thank you! Just merged, updated, and fixed some other warnings.

---

_@charliermarsh approved on 2023-09-18 23:53_

---

_Merged by @charliermarsh on 2023-09-19 00:01_

---

_Closed by @charliermarsh on 2023-09-19 00:01_

---

_Branch deleted on 2023-09-19 12:29_

---
