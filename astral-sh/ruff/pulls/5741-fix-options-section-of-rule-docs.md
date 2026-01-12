```yaml
number: 5741
title: "Fix `Options` section of rule docs"
type: pull_request
state: merged
author: eggplants
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-option-anchor
created_at: 2023-07-13T16:51:29Z
updated_at: 2023-07-14T00:28:26Z
url: https://github.com/astral-sh/ruff/pull/5741
synced_at: 2026-01-12T03:30:21Z
```

# Fix `Options` section of rule docs

---

_Pull request opened by @eggplants on 2023-07-13 16:51_

## Summary

Fix: #5740

A trailing line-break is needed for the anchor.

## Test Plan

http://127.0.0.1:8000/docs/rules/line-too-long/#options

|before|after|
|--|--|
|![image](https://github.com/astral-sh/ruff/assets/42153744/8cb9dcce-aeda-4255-b21e-ab11817ba9e1)|![image](https://github.com/astral-sh/ruff/assets/42153744/b68d4fd7-da5a-4494-bb95-f7792f1a42db)|

---

_Comment by @charliermarsh on 2023-07-13 16:52_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2023-07-13 16:52_

---

_Merged by @charliermarsh on 2023-07-13 17:25_

---

_Closed by @charliermarsh on 2023-07-13 17:25_

---

_Comment by @github-actions[bot] on 2023-07-13 17:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.04     10.0±0.53ms     4.1 MB/sec     1.00      9.6±0.40ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.14ms     7.4 MB/sec     1.03      2.3±0.16ms     7.2 MB/sec
formatter/numpy/globals.py                 1.01   260.4±23.18µs    11.3 MB/sec     1.00   258.0±24.47µs    11.4 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.33ms     5.2 MB/sec     1.02      5.0±0.37ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.9±0.79ms     2.4 MB/sec     1.00     16.9±0.63ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.24ms     4.0 MB/sec     1.02      4.3±0.25ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   561.2±42.95µs     5.3 MB/sec     1.01   569.0±39.66µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.47ms     3.4 MB/sec     1.00      7.5±0.36ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.33ms     4.9 MB/sec     1.01      8.3±0.34ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1772.3±104.23µs     9.4 MB/sec    1.01  1792.1±118.45µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.04   215.4±19.21µs    13.7 MB/sec     1.00    207.2±8.52µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.18ms     6.8 MB/sec     1.06      4.0±0.27ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     12.7±0.38ms     3.2 MB/sec    1.00     12.6±0.37ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.8±0.09ms     5.9 MB/sec    1.02      2.9±0.13ms     5.8 MB/sec
formatter/numpy/globals.py                 1.00   324.3±19.03µs     9.1 MB/sec    1.05   339.3±32.77µs     8.7 MB/sec
formatter/pydantic/types.py                1.00      6.1±0.21ms     4.2 MB/sec    1.01      6.2±0.21ms     4.1 MB/sec
linter/all-rules/large/dataset.py          1.00     21.0±0.48ms  1986.3 KB/sec    1.00     20.9±0.42ms  1988.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.17ms     3.0 MB/sec    1.00      5.5±0.17ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   671.3±42.64µs     4.4 MB/sec    1.01   680.5±38.41µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.5±0.26ms     2.7 MB/sec    1.00      9.4±0.25ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.7±0.23ms     3.8 MB/sec    1.00     10.7±0.22ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.08ms     7.3 MB/sec    1.02      2.3±0.11ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    276.7±9.99µs    10.7 MB/sec    1.01   279.9±12.11µs    10.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.13ms     5.3 MB/sec    1.01      4.9±0.21ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-07-13 17:31_

---
