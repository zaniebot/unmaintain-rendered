```yaml
number: 5847
title: "refactor: use find_keyword ast helper more"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: refactor-use-helper-find-keyword
created_at: 2023-07-17T23:22:54Z
updated_at: 2023-07-18T00:37:40Z
url: https://github.com/astral-sh/ruff/pull/5847
synced_at: 2026-01-12T03:30:21Z
```

# refactor: use find_keyword ast helper more

---

_Pull request opened by @sbrugman on 2023-07-17 23:22_

Use the ast helper function `find_keyword` where applicable

(found these while working on another feature)

---

_@charliermarsh approved on 2023-07-17 23:23_

---

_Comment by @github-actions[bot] on 2023-07-17 23:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.04ms     4.1 MB/sec    1.00      9.9±0.05ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.01   1897.8±3.41µs     8.8 MB/sec    1.00   1886.2±4.09µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    206.1±0.81µs    14.3 MB/sec    1.00    205.2±1.44µs    14.4 MB/sec
formatter/pydantic/types.py                1.02      4.3±0.06ms     6.0 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.8±0.04ms     3.0 MB/sec    1.01     14.0±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    374.2±1.47µs     7.9 MB/sec    1.00    375.8±2.08µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.05ms     4.1 MB/sec    1.02      6.3±0.16ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.02ms     5.9 MB/sec    1.02      7.1±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1404.2±1.85µs    11.9 MB/sec    1.01   1418.5±1.98µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    148.4±7.63µs    19.9 MB/sec    1.00    147.5±0.25µs    20.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.02      3.1±0.01ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.0±0.21ms     3.1 MB/sec    1.00     13.1±0.25ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.08ms     6.5 MB/sec    1.00      2.6±0.11ms     6.4 MB/sec
formatter/numpy/globals.py                 1.00   289.9±10.21µs    10.2 MB/sec    1.04   300.1±19.00µs     9.8 MB/sec
formatter/pydantic/types.py                1.00      5.7±0.16ms     4.5 MB/sec    1.00      5.7±0.14ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.01     18.4±0.25ms     2.2 MB/sec    1.00     18.3±0.23ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.9±0.11ms     3.4 MB/sec    1.00      4.8±0.11ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   593.2±15.36µs     5.0 MB/sec    1.00   591.9±16.01µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.4±0.15ms     3.0 MB/sec    1.00      8.3±0.14ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.01      9.5±0.13ms     4.3 MB/sec    1.00      9.4±0.17ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.0±0.04ms     8.3 MB/sec    1.00  1970.2±36.68µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    230.6±7.92µs    12.8 MB/sec    1.00    228.9±6.49µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.3±0.17ms     6.0 MB/sec    1.00      4.2±0.10ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-17 23:37_

---

_Closed by @charliermarsh on 2023-07-17 23:37_

---

_Branch deleted on 2023-07-18 00:37_

---
