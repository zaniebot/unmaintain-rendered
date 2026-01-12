```yaml
number: 6794
title: Remove comment lexing from isort
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lex
created_at: 2023-08-22T21:19:49Z
updated_at: 2023-08-22T21:55:20Z
url: https://github.com/astral-sh/ruff/pull/6794
synced_at: 2026-01-12T15:55:22Z
```

# Remove comment lexing from isort

---

_@charliermarsh_

## Summary

No need to lex to find comments -- we already know their locations via `Indexer`.

---

_Label `internal` added by @charliermarsh on 2023-08-22 21:19_

---

_Merged by @charliermarsh on 2023-08-22 21:26_

---

_Closed by @charliermarsh on 2023-08-22 21:26_

---

_Branch deleted on 2023-08-22 21:26_

---

_Comment by @github-actions[bot] on 2023-08-22 21:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.01ms    12.2 MB/sec    1.00      3.3±0.01ms    12.3 MB/sec
formatter/numpy/ctypeslib.py               1.01    680.4±5.58µs    24.5 MB/sec    1.00    676.9±7.47µs    24.6 MB/sec
formatter/numpy/globals.py                 1.00     71.4±0.25µs    41.3 MB/sec    1.00     71.4±0.40µs    41.3 MB/sec
formatter/pydantic/types.py                1.01  1357.3±29.41µs    18.8 MB/sec    1.00  1348.8±12.24µs    18.9 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.01ms     3.9 MB/sec    1.00     10.4±0.02ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.00ms     5.8 MB/sec    1.00      2.9±0.01ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    321.6±0.85µs     9.2 MB/sec    1.00    319.7±0.72µs     9.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.01ms     4.7 MB/sec    1.00      5.4±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.01      5.5±0.02ms     7.3 MB/sec    1.00      5.5±0.01ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1195.6±2.82µs    13.9 MB/sec    1.00   1186.2±5.96µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    123.7±0.25µs    23.9 MB/sec    1.00    123.4±0.34µs    23.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.00ms    10.1 MB/sec    1.00      2.5±0.01ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      4.3±0.18ms     9.4 MB/sec    1.00      4.3±0.31ms     9.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   906.2±53.21µs    18.4 MB/sec    1.00   901.8±60.33µs    18.5 MB/sec
formatter/numpy/globals.py                 1.01     94.2±5.13µs    31.3 MB/sec    1.00     93.1±5.71µs    31.7 MB/sec
formatter/pydantic/types.py                1.00  1823.2±81.27µs    14.0 MB/sec    1.00  1817.4±117.27µs    14.0 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.46ms     2.7 MB/sec    1.01     15.4±0.49ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.14ms     4.0 MB/sec    1.02      4.2±0.16ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   527.8±26.05µs     5.6 MB/sec    1.03   543.3±25.65µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.1±0.28ms     3.2 MB/sec    1.00      8.0±0.23ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.16ms     4.8 MB/sec    1.03      8.7±0.34ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1794.3±62.70µs     9.3 MB/sec    1.03  1851.4±82.34µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.03    207.4±8.41µs    14.2 MB/sec    1.00   202.0±14.61µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.17ms     6.7 MB/sec    1.01      3.9±0.15ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
