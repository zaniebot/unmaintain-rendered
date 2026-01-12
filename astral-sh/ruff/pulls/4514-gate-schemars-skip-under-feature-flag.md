```yaml
number: 4514
title: "Gate `schemars` skip under feature flag"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/skip
created_at: 2023-05-19T02:54:16Z
updated_at: 2023-05-19T03:22:08Z
url: https://github.com/astral-sh/ruff/pull/4514
synced_at: 2026-01-12T15:55:15Z
```

# Gate `schemars` skip under feature flag

---

_@charliermarsh_

This snuck in due to merging against a non-updated main.

---

_Renamed from "Gate `schemars` skip under fetaure flag" to "Gate `schemars` skip under feature flag" by @charliermarsh on 2023-05-19 02:54_

---

_Merged by @charliermarsh on 2023-05-19 03:01_

---

_Closed by @charliermarsh on 2023-05-19 03:01_

---

_Branch deleted on 2023-05-19 03:01_

---

_Comment by @github-actions[bot] on 2023-05-19 03:22_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.83ms     2.5 MB/sec    1.05     17.2±0.97ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.26ms     4.2 MB/sec    1.03      4.1±0.18ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   517.5±24.52µs     5.7 MB/sec    1.03   530.5±19.72µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.31ms     3.8 MB/sec    1.08      7.2±0.28ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.38ms     5.3 MB/sec    1.01      7.8±0.33ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1643.3±69.28µs    10.1 MB/sec    1.06  1747.6±71.41µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   209.6±16.88µs    14.1 MB/sec    1.00   209.7±25.42µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.6±0.18ms     7.1 MB/sec    1.00      3.5±0.16ms     7.3 MB/sec
parser/large/dataset.py                    1.04      6.5±0.28ms     6.3 MB/sec    1.00      6.2±0.29ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.01  1241.9±57.71µs    13.4 MB/sec    1.00  1231.2±51.04µs    13.5 MB/sec
parser/numpy/globals.py                    1.03    125.4±9.11µs    23.5 MB/sec    1.00    122.2±5.83µs    24.1 MB/sec
parser/pydantic/types.py                   1.06      2.8±0.10ms     9.1 MB/sec    1.00      2.6±0.09ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.22ms     2.4 MB/sec    1.04     17.4±0.38ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     3.9 MB/sec    1.03      4.3±0.08ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.0±9.12µs     5.9 MB/sec    1.00    497.4±7.41µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.6 MB/sec    1.03      7.2±0.12ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.11ms     4.9 MB/sec    1.01      8.4±0.13ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1769.5±22.33µs     9.4 MB/sec    1.00  1761.6±34.68µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.6±4.01µs    15.0 MB/sec    1.01    198.5±7.61µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.04ms     6.7 MB/sec    1.00      3.7±0.04ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.7±0.08ms     6.1 MB/sec    1.00      6.8±0.05ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1269.0±12.40µs    13.1 MB/sec    1.02  1290.9±19.61µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    131.8±2.98µs    22.4 MB/sec    1.02    133.9±6.84µs    22.0 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.0 MB/sec    1.01      2.9±0.04ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
