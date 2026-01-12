```yaml
number: 5533
title: "Use structs for noqa `Directive` variants"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/noqa
created_at: 2023-07-05T16:05:22Z
updated_at: 2023-07-05T21:52:53Z
url: https://github.com/astral-sh/ruff/pull/5533
synced_at: 2026-01-12T15:55:18Z
```

# Use structs for noqa `Directive` variants

---

_@charliermarsh_

## Summary

No behavioral changes, just clearer (IMO) and with better documentation.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-05 16:05_

---

_Comment by @github-actions[bot] on 2023-07-05 16:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.4±0.27ms     3.9 MB/sec    1.03     10.7±0.29ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.08ms     7.4 MB/sec    1.02      2.3±0.08ms     7.3 MB/sec
formatter/numpy/globals.py                 1.03   265.5±40.04µs    11.1 MB/sec    1.00   259.0±10.39µs    11.4 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.20ms     5.1 MB/sec    1.00      5.0±0.18ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     19.2±0.42ms     2.1 MB/sec    1.01     19.4±0.59ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.7±0.66ms     3.5 MB/sec    1.00      4.6±0.21ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   579.5±31.08µs     5.1 MB/sec    1.03   595.2±34.62µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.25ms     3.1 MB/sec    1.02      8.5±0.40ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.01      9.4±0.34ms     4.3 MB/sec    1.00      9.3±0.26ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1915.2±64.71µs     8.7 MB/sec    1.02  1956.9±76.00µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   227.6±11.81µs    13.0 MB/sec    1.01   231.0±12.92µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.18ms     6.2 MB/sec    1.02      4.2±0.18ms     6.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.73ms     3.7 MB/sec    1.10     12.2±0.60ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.17ms     7.3 MB/sec    1.09      2.5±0.16ms     6.7 MB/sec
formatter/numpy/globals.py                 1.00   271.8±25.63µs    10.9 MB/sec    1.11   301.0±20.99µs     9.8 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.32ms     4.8 MB/sec    1.01      5.3±0.25ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.05     19.8±0.70ms     2.1 MB/sec    1.00     18.7±1.09ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.19ms     3.3 MB/sec    1.02      5.2±0.33ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.03   646.3±43.44µs     4.6 MB/sec    1.00   625.2±35.99µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.26ms     3.0 MB/sec    1.08      9.1±0.37ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.15     11.7±2.35ms     3.5 MB/sec    1.00     10.2±0.42ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.29ms     8.1 MB/sec    1.06      2.2±0.08ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   248.2±38.07µs    11.9 MB/sec    1.13   279.6±15.43µs    10.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.41ms     5.6 MB/sec    1.04      4.8±0.19ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `internal` added by @charliermarsh on 2023-07-05 21:30_

---

_Renamed from "Use structs for noqa Directive variants" to "Use structs for noqa `Directive` variants" by @charliermarsh on 2023-07-05 21:30_

---

_Merged by @charliermarsh on 2023-07-05 21:37_

---

_Closed by @charliermarsh on 2023-07-05 21:37_

---

_Branch deleted on 2023-07-05 21:37_

---
