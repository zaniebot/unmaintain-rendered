```yaml
number: 4383
title: Isolate show statistic integration test
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: isolate-show-statistic-integration-test
created_at: 2023-05-11T21:26:59Z
updated_at: 2023-05-11T22:09:38Z
url: https://github.com/astral-sh/ruff/pull/4383
synced_at: 2026-01-12T15:55:15Z
```

# Isolate show statistic integration test

---

_@JonathanPlasse_

Just found out that this test failed with the global config I had on my old computer.

---

_Merged by @charliermarsh on 2023-05-11 21:42_

---

_Closed by @charliermarsh on 2023-05-11 21:42_

---

_Comment by @github-actions[bot] on 2023-05-11 21:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.01     16.6±0.45ms     2.5 MB/sec     1.00     16.5±0.35ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.11ms     4.2 MB/sec     1.02      4.1±0.16ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   513.2±29.38µs     5.7 MB/sec     1.02   521.5±31.42µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.21ms     3.7 MB/sec     1.00      6.9±0.24ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.20ms     5.1 MB/sec     1.00      7.9±0.20ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1745.4±119.08µs     9.5 MB/sec    1.00  1679.6±55.32µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.1±6.13µs    14.5 MB/sec     1.00    204.5±7.77µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.29ms     6.9 MB/sec     1.00      3.6±0.13ms     7.1 MB/sec
parser/large/dataset.py                    1.01      6.5±0.16ms     6.3 MB/sec     1.00      6.4±0.17ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1278.5±44.86µs    13.0 MB/sec     1.00  1275.8±70.25µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    127.6±5.00µs    23.1 MB/sec     1.00    128.0±6.57µs    23.1 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.12ms     9.1 MB/sec     1.00      2.8±0.11ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     21.3±0.83ms  1958.1 KB/sec     1.01     21.4±0.74ms  1944.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.15ms     3.1 MB/sec     1.00      5.3±0.21ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   634.9±25.05µs     4.6 MB/sec     1.00   632.2±47.81µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.40ms     2.9 MB/sec     1.01      9.0±0.48ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.28ms     4.0 MB/sec     1.00     10.3±0.43ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.6 MB/sec     1.00      2.2±0.06ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   256.2±10.09µs    11.5 MB/sec     1.05   268.5±40.22µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.20ms     5.4 MB/sec     1.01      4.7±0.24ms     5.4 MB/sec
parser/large/dataset.py                    1.00      8.4±0.27ms     4.8 MB/sec     1.00      8.4±0.25ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1651.7±108.09µs    10.1 MB/sec    1.02  1683.9±118.80µs     9.9 MB/sec
parser/numpy/globals.py                    1.00    161.1±5.89µs    18.3 MB/sec     1.01    163.2±7.51µs    18.1 MB/sec
parser/pydantic/types.py                   1.00      3.5±0.12ms     7.3 MB/sec     1.02      3.6±0.10ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-05-11 21:43_

---
