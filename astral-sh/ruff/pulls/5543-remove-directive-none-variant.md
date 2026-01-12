```yaml
number: 5543
title: "Remove `Directive::None` variant"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/noqa-none
created_at: 2023-07-05T22:16:17Z
updated_at: 2023-07-05T22:44:17Z
url: https://github.com/astral-sh/ruff/pull/5543
synced_at: 2026-01-12T15:55:18Z
```

# Remove `Directive::None` variant

---

_@charliermarsh_

## Summary

This is creating some weird, impossible states. Make impossible states unrepresentable!

---

_Label `internal` added by @charliermarsh on 2023-07-05 22:19_

---

_Merged by @charliermarsh on 2023-07-05 22:22_

---

_Closed by @charliermarsh on 2023-07-05 22:22_

---

_Branch deleted on 2023-07-05 22:22_

---

_Comment by @github-actions[bot] on 2023-07-05 22:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.03ms     4.3 MB/sec    1.00      9.4±0.03ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.00ms     8.0 MB/sec    1.00      2.1±0.00ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    233.8±1.02µs    12.6 MB/sec    1.00    234.2±0.89µs    12.6 MB/sec
formatter/pydantic/types.py                1.01      4.6±0.01ms     5.6 MB/sec    1.00      4.5±0.01ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     16.2±0.07ms     2.5 MB/sec    1.00     16.2±0.08ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.1 MB/sec    1.00      4.0±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    513.0±0.82µs     5.8 MB/sec    1.00    512.6±1.94µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.06ms     3.6 MB/sec    1.00      7.1±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.03ms     5.1 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1748.8±3.62µs     9.5 MB/sec    1.00   1739.5±4.73µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.8±0.32µs    15.1 MB/sec    1.00    196.2±1.63µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.4±0.44ms     3.9 MB/sec    1.06     11.0±0.86ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.3±0.11ms     7.2 MB/sec    1.00      2.2±0.11ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   271.5±13.34µs    10.9 MB/sec    1.06   286.7±21.78µs    10.3 MB/sec
formatter/pydantic/types.py                1.07      5.2±0.21ms     4.9 MB/sec    1.00      4.9±0.27ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     16.9±0.52ms     2.4 MB/sec    1.01     17.0±0.78ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.10      4.9±0.29ms     3.4 MB/sec    1.00      4.5±0.17ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   547.6±24.97µs     5.4 MB/sec    1.11   606.6±61.48µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.6±0.35ms     3.3 MB/sec    1.00      7.5±0.38ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.27ms     4.7 MB/sec    1.00      8.7±0.30ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1856.4±81.46µs     9.0 MB/sec    1.01  1872.1±63.03µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   241.9±18.68µs    12.2 MB/sec    1.03   250.4±25.00µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.3±0.22ms     5.9 MB/sec    1.00      4.1±0.21ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
