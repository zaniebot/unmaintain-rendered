```yaml
number: 5759
title: "Add Regression test for #5605, where formatting `x[:,]` failed."
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: test_for_5605
created_at: 2023-07-14T09:12:36Z
updated_at: 2023-07-14T09:55:06Z
url: https://github.com/astral-sh/ruff/pull/5759
synced_at: 2026-01-12T03:30:21Z
```

# Add Regression test for #5605, where formatting `x[:,]` failed.

---

_Pull request opened by @konstin on 2023-07-14 09:12_

#5605 has been fixed, i added the failing example from the issue as a regression test.

Closes #5605

---

_Label `formatter` added by @konstin on 2023-07-14 09:12_

---

_Comment by @github-actions[bot] on 2023-07-14 09:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.26ms     4.0 MB/sec    1.01     10.3±0.21ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.07ms     8.2 MB/sec    1.05      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00   229.0±10.16µs    12.9 MB/sec    1.06    243.1±2.97µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.12ms     5.6 MB/sec    1.03      4.7±0.05ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.11     16.3±0.34ms     2.5 MB/sec    1.00     14.6±0.62ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.9±0.08ms     4.2 MB/sec    1.00      3.8±0.14ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   512.8±14.98µs     5.8 MB/sec    1.00    513.9±9.92µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.15ms     3.8 MB/sec    1.03      6.9±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.16ms     5.3 MB/sec    1.01      7.7±0.12ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1627.5±87.36µs    10.2 MB/sec    1.06  1719.1±21.61µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.2±5.88µs    15.9 MB/sec    1.04    193.5±4.81µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.11ms     7.4 MB/sec    1.01      3.5±0.05ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.4±0.60ms     3.6 MB/sec    1.00     11.2±0.54ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.3±0.13ms     7.3 MB/sec    1.00      2.2±0.15ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   258.9±22.53µs    11.4 MB/sec    1.00   259.8±22.04µs    11.4 MB/sec
formatter/pydantic/types.py                1.01      4.9±0.25ms     5.2 MB/sec    1.00      4.8±0.30ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     16.8±0.64ms     2.4 MB/sec    1.00     16.5±0.69ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.35ms     3.9 MB/sec    1.00      4.3±0.23ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   523.1±31.54µs     5.6 MB/sec    1.02   531.0±34.84µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.39ms     3.5 MB/sec    1.03      7.6±0.33ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.31ms     4.8 MB/sec    1.00      8.4±0.32ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1844.3±83.94µs     9.0 MB/sec    1.00  1780.8±95.20µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.01   214.7±14.68µs    13.7 MB/sec    1.00   212.6±13.44µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.18ms     6.8 MB/sec    1.00      3.8±0.17ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-14 09:25_

---

_Merged by @konstin on 2023-07-14 09:55_

---

_Closed by @konstin on 2023-07-14 09:55_

---

_Branch deleted on 2023-07-14 09:55_

---
